# Prueba Técnica SMG – Shopify Custom Theme
Este repositorio contiene el desarrollo de un tema personalizado para Shopify enfocado en la venta de servicios digitales, cumpliendo los siguientes requerimientos:

- Página de inicio que muestra una colección de servicios.
- Integración AJAX para añadir productos al carrito con un regalo incluido.
- Lógica de carrito que restringe la compra a un solo servicio por vez.
- Estilo visual minimalista y adaptado al diseño del tema base.

---

## 🚀 Instrucciones de uso

1. Clona el repositorio:
```bash
git clone https://github.com/JavierAChacon/prueba-tecnica-smg.git
```
2. Abre el proyecto en tu editor y conéctalo a tu tienda Shopify con el Shopify CLI.
3. Para probar localmente:
```bash
shopify theme dev
```
---

## 🧩 Documentación técnica

### 🧱 Esquema (schema) de la página de productos
El archivo `servicios.liquid` contiene el schema necesario para configurar visualmente la página en el editor de Shopify:

```liquid
{% schema %}
{
  "name": "Servicios",
  "settings": [
    {
      "type": "collection",
      "id": "servicios_collection",
      "label": "Colección de servicios"
    },
    {
      "type": "product",
      "id": "producto_regalo",
      "label": "Producto de regalo"
    }
  ],
  "presets": [
    {
      "name": "Servicios"
    }
  ]
}
{% endschema %}
```

### ⚙️ AJAX Add to Cart
En el archivo `servicios.liquid` se implementó un flujo de compra con JavaScript que:

- Limpia el carrito (limitando la compra a 1 servicio).
- Agrega el servicio seleccionado.
- Agrega automáticamente el producto de regalo.

```javascript
await fetch('/cart/clear.js', { method: 'POST' });

await fetch('/cart/add.js', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ id: variantId, quantity: 1 }),
});

await fetch('/cart/add.js', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ id: regaloId, quantity: 1 }),
});
```

### ❌ Restricción de múltiples servicios
La lógica del carrito impide que el usuario agregue más de un servicio a la vez. Esto se logra utilizando la ruta:
```javascript
await fetch('/cart/clear.js', { method: 'POST' });
```
Cada vez que un usuario selecciona un nuevo servicio, el carrito se limpia antes de agregar el producto y su respectivo regalo. Esto garantiza:

✅ Que el usuario solo tenga un servicio activo por vez en el carrito.

🎁 Que cada servicio venga acompañado de su producto de regalo.

🔒 Que el regalo no pueda eliminarse directamente desde el carrito.

### 📂 Estructura relevante
```bash
/sections
  └── servicios.liquid        # Página principal de productos
/templates
  └── index.liquid            # Página de inicio
/snippets
  └── cart.liquid             # Carrito con lógica condicional
```

### 🔐 Requisitos de CI y CLA

- Se utiliza `theme-check` como validación de CI.
- CLA configurado para confirmar contribuciones firmadas.
- GitHub Actions habilitadas en `.github/workflows`.
