---
theme: seriph
background: https://images.unsplash.com/photo-1653406759381-4d6ca279cab3?q=80&w=687&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
title: Figma MCP - Turn your Figma Design to code
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
seoMeta:
  ogImage: auto
layout: cover
hideInToc: true

---

# Figma MCP 

Turn your Figma Design to code

PHNam

--- 

<Toc text-sm list-class="text-primary" minDepth="1" maxDepth="1" />

---

# 📡 Model Context Protocol

- **Model Context Protocol (MCP)** là một giao thức mở giúp tích hợp liền mạch giữa các ứng dụng LLM với các nguồn dữ liệu và công cụ từ các ứng dụng bên ngoài.
- Hoạt động theo kiến trúc client-host-server

<img class="mcp-architecture mt-4" src="./images/mcp-architecture.png" />

<style>
.mcp-architecture {
  height: 300px;
  width: auto;
  display: block;
  margin-inline: auto;
}
</style>

---

# ℹ️ Giới thiệu Figma MCP

<ul>
<li>

Tính năng <span v-mark.circle.red="1">(BETA)</span> cho phép kết nối trực tiếp với công cụ lập trình.
[Give Feedback](https://form.asana.com/?k=jMdFq_1SBUOyh8_k3q76QA&d=10497086658021/)

</li>

<li>
  Cung cấp <span v-mark.underline.red="2">ngữ cảnh thiết kế(design context)</span> từ Figma cho LLM
</li>

<img class="figma-mcp mt-6 center" src="https://miro.medium.com/v2/resize:fit:1400/1*TOiXt8_tZ7UShX3r6kjxVQ.png">
</ul>

<style>
.figma-mcp {
  height: 300px;
  width: auto;
  display: block;
  margin-inline: auto;
}
</style>
---

# ⚙️ Yêu cầu ban đầu

- Figma Desktop đăng nhập tài khoản quyền Dev Mode/Full seat
- VS Code + GitHub Copilot Chat
- Project Vue/Nuxt sẵn có
- File thiết kế Figma

---
layout: two-cols
layoutClass: gap-8
---

# 🟢 Kích hoạt MCP Server

1. Mở file design bất kỳ với Figma Desktop
2. Menu > Preferences > Enable Dev Mode MCP Server
3. Kiểm tra: http://127.0.0.1:3845/mcp

::right::

<div class="img-container mt-6" v-click>
  <div class="image-wrapper"></div>
</div>

<style>
.img-container {
  width: 600px;
  height: 480px;
  overflow: hidden;
}

.image-wrapper {
  width: 100%;
  height: 100%;
  background-image: url('./images/enable_figma_mcp_server.gif');
  background-position: center center;
  background-repeat: no-repeat;
  background-size: 100%;
}

</style>
---

# 🖥️ Cấu hình VSCode

- Cài GitHub Copilot Chat
- `settings.json`:

```json
"chat.mcp.discovery.enabled": true,
"chat.agent.enabled": true
"mcp": {
  "servers": {
    "Figma Dev Mode MCP": {
      "type": "sse",
      "url": "http://127.0.0.1:3845/mcp"
    }
  }
},
```

---

# 🛠️ Các công cụ của Figma MCP


<ul>

<li v-click><code>get_code</code>: Generate code của node.</li>
<li v-click><code>get_image</code>: Trả về ảnh chụp của node.</li>
<li v-click><code>get_variable_defs</code>: Trả về các biến và kiểu được sử dụng như màu sắc, khoảng cách và kiểu chữ.</li>
<li v-click><code>get_code_connect_map</code>: Trả về ánh xạ giữ figma node và component tương ứng trong source code.</li>
<li v-click><code>create_design_system_rules</code>: Tạo tệp quy tắc(rules) để cung cấp cho AI Agent.</li>

</ul>

---

# 🤖 Dùng AI sinh mã giao diện

- Mở Copilot Chat > Agent Mode
- Prompt ví dụ:

```txt
Generate a Vue 3 component for the selected Figma frame. 
Use <script setup> and try to break it into sub-components as much as possible.
```

---

# 📦 Kết quả nhận được

- Một hoặc nhiều file `.vue` gồm:
  - `<template>` đúng bố cục Figma
  - `<script setup>` đã định nghĩa props

---

# ✅ Tối ưu prompt hiệu quả

- Nói rõ công nghệ: Vue/Nuxt
- Chia nhỏ từng component
- Yêu cầu dùng component có sẵn nếu có

Example: 

```txt
Generate Nuxt 3 (Vue 3) components from selection, split into reusable parts, 
reuse existing components, follow project styles, clean code.
```

---

# 🔁 Tái sử dụng với Code Connect

- Mapping NodeID <-> Component Codebase
- AI sẽ dùng `<BaseButton>` thay vì `<button>`
- Dùng biến thiết kế (tokens) thay vì hard-code

---

# 🧠 Best Practices

- Dùng [Component](https://help.figma.com/hc/en-us/articles/360038662654-Guide-to-components-in-Figma) + [Auto Layout](https://help.figma.com/hc/en-us/articles/360040451373-Guide-to-auto-layout) + đặt tên lớp/layer có ý nghĩa
- Dùng biến thiết kế ([Variables](https://help.figma.com/hc/en-us/articles/15339657135383-Guide-to-variables-in-Figma))
- Ghi chú [Dev Mode (annotation)](https://help.figma.com/hc/en-us/articles/20774752502935-Add-measurements-and-annotate-designs)
- Viết prompt mô tả rõ yêu cầu, mong muốn
- Chạy thử & chỉnh sửa code nếu cần

---

# 🧪 Minh hoạ: ProductCard.vue

```vue
<template>
  <div class="product-card">
    <img src="/path-to-product-image" />
    <h2 class="name">{{ productName }}</h2>
    <p class="price">{{ price }}</p>
    <button class="button -buy">Buy Now</button>
  </div>
</template>
<script setup>
defineProps({ productName: String, price: String })
</script>
```

---

# 🎯 Kết luận

- MCP giúp AI hiểu đúng thiết kế
- Tăng tốc & đồng bộ Dev/Design
- Bắt đầu từ nhỏ → mở rộng dần
- Duy trì mapping, cập nhật khi thiết kế đổi

---

# 📚 Tham khảo

- [Model Context Protocol](https://modelcontextprotocol.io/specification/2025-06-18)
- [Figma MCP Docs](https://help.figma.com/hc/en-us/articles/32132100833559)
- [Vue School MCP Guide](https://vueschool.io/articles/vuejs-tutorials/the-model-context-protocol-mcp-for-web-developers/)
- [Figma Blog](https://www.figma.com/blog/introducing-figmas-dev-mode-mcp-server/)

<PoweredBySlidev mt-10 />
