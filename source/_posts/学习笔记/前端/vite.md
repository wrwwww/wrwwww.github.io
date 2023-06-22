---
title: vite
date: 2023-05-05 21:30:42
tags:  
---
## 路径别名
```typescript
// 在vite配置中
// 需要导入依赖 import { fileURLToPath, URL } from "node:url";

 resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
```