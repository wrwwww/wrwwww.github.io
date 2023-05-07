## 在vite项目中导入
```shell
# 添加预处理的依赖
npm add -D sass
```
```vue
<!-- 在单文件组件中打开设置-->
<style lang="scss" scoped>
</style>

```
```typescript
// 在vite的配置中添加配置
// 以便在全局使用定义的scss变量
css: {
    preprocessorOptions: {
      //define global scss variable
      scss: {
        additionalData: `@import "@/style/_constant.scss";`
      }
    }
  }
```
