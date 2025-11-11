# Community Page - Testimonials Component

Vue 3 项目，包含客户评价/社区页面组件。

## 功能特性

- ✅ Vue 3 Composition API
- ✅ SCSS 嵌套样式结构
- ✅ 响应式设计（桌面端 2 列，移动端单列）
- ✅ 客户评价卡片网格布局
- ✅ 渐变文本效果
- ✅ 导航按钮（带渐变背景）
- ✅ 星级评分系统
- ✅ 圆形头像（带边框）

## 安装依赖

```bash
npm install
```

## 运行开发服务器

```bash
npm run dev
```

## 构建生产版本

```bash
npm run build
```

## 预览生产构建

```bash
npm run preview
```

## 项目结构

```
.
├── src/
│   ├── components/
│   │   └── TestimonialsComponent.vue  # 主组件
│   ├── App.vue                        # 根组件
│   ├── main.js                        # 入口文件
│   └── style.css                      # 全局样式
├── index.html                         # HTML 模板
├── package.json                       # 项目配置
└── vite.config.js                     # Vite 配置
```

## 组件说明

`TestimonialsComponent.vue` 包含：

- **标题区域**: "What Our Clients Say?" 主标题（带渐变文本效果）和 "100k Clients In 2023" 副标题
- **导航按钮**: 左右箭头按钮，带有渐变背景
- **评价卡片网格**: 
  - 桌面端：2x2 网格布局
  - 移动端：单列堆叠
  - 每个卡片包含：引用图标、评价内容、客户信息（头像、姓名、职位、评分）

## 自定义数据

可以在 `TestimonialsComponent.vue` 的 `testimonials` 数组中修改客户评价数据。

