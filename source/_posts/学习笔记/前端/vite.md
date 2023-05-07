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