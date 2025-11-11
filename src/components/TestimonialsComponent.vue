<template>
  <div class="testimonials-container">
    <!-- 头部区域：包含副标题、主标题和导航按钮 -->
    <div class="header-section">
      <!-- 左上角副标题 -->
      <div class="subtitle">· 100k Clients In 2023</div>
      
      <!-- 居中主标题，包含渐变文本效果 -->
      <div class="title-wrapper">
        <h1 class="main-title">
          What Our <span class="gradient-text">Clients</span> Say?
        </h1>
      </div>
      
      <!-- 右侧导航按钮组 -->
      <div class="navigation-buttons">
        <!-- 上一个按钮 -->
        <button class="nav-btn prev-btn" @click="handlePrev" aria-label="Previous testimonial">
          <svg width="17" height="17" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M15 18L9 12L15 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <!-- 下一个按钮 -->
        <button class="nav-btn next-btn" @click="handleNext" aria-label="Next testimonial">
          <svg width="17" height="17" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M9 18L15 12L9 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
    </div>

    <!-- 评价卡片网格容器 -->
    <div class="testimonials-grid" :class="{ 'is-animating': isAnimating }">
      <!-- 遍历渲染当前显示的4张评价卡片 -->
      <transition-group 
        :name="slideDirection" 
        tag="div" 
        class="testimonials-grid-inner"
      >
        <div
          v-for="(testimonial, index) in displayedTestimonials"
          :key="testimonial.name + currentStartIndex"
          class="testimonial-card"
          :data-card-index="index"
        >
        <!-- 装饰性引用标记（左上角大引号） -->
        <div class="quotation-mark">“</div>
        
        <!-- 客户评价内容 -->
        <p class="testimonial-quote">{{ testimonial.quote }}</p>
        
        <!-- 客户信息区域 -->
        <div class="customer-info">
          <!-- 客户头像 -->
          <div class="avatar-wrapper">
            <img
              :src="testimonial.avatarUrl"
              :alt="testimonial.name"
              class="avatar"
            />
          </div>
          
          <!-- 客户详细信息 -->
          <div class="customer-details">
            <!-- 第一行：客户姓名和星级评分 -->
            <div class="name-rating-row">
              <h3 class="customer-name">{{ testimonial.name }}</h3>
              <!-- 星级评分（5颗星） -->
              <div class="rating">
                <span
                  v-for="star in 5"
                  :key="star"
                  class="star"
                  :class="{ filled: star <= testimonial.rating }"
                >
                  ★
                </span>
              </div>
            </div>
            <!-- 第二行：客户职业/职位 -->
            <p class="customer-title">{{ testimonial.title }}</p>
          </div>
        </div>
      </div>
      </transition-group>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

/**
 * 客户评价数据数组（完整数据集）
 * 每个评价对象包含：
 * @property {string} quote - 客户评价内容
 * @property {string} name - 客户姓名
 * @property {string} title - 客户职业/职位
 * @property {string} avatarUrl - 客户头像图片URL
 * @property {number} rating - 客户评分（1-5星）
 */
const allTestimonials = [
  {
    quote: 'hdrbchsbchdf bhwbcd cbwcblaCSJBWAIWSBA CIbn cbdirv scjhdbncnsibvndjikbvswujdbovsl nsjvudjs ncdsbdjcvdjnbsvznldbazie vbalzuws.',
    name: 'Jobs Benjamin',
    title: 'Managment',
    avatarUrl: 'https://i.pravatar.cc/150?img=1',
    rating: 5
  },
  {
    quote: 'hdrbchsbchdf bhwbcd cbwcblaCSJBWAIWSBA CIbn cbdirv scjhdbncnsibvndjikbvswujdbovsl nsjvudjs ncdsbdjcvdjnbsvznldbazie vbalzuws.',
    name: 'John Esme',
    title: 'Lawer',
    avatarUrl: 'https://i.pravatar.cc/150?img=2',
    rating: 5
  },
  {
    quote: 'hdrbchsbchdf bhwbcd cbwcblaCSJBWAIWSBA CIbn cbdirv scjhdbncnsibvndjikbvswujdbovsl nsjvudjs ncdsbdjcvdjnbsvznldbazie vbalzuws.',
    name: 'Simth Felix',
    title: 'Invester',
    avatarUrl: 'https://i.pravatar.cc/150?img=3',
    rating: 5
  },
  {
    quote: 'hdrbchsbchdf bhwbcd cbwcblaCSJBWAIWSBA CIbn cbdirv scjhdbncnsibvndjikbvswujdbovsl nsjvudjs ncdsbdjcvdjnbsvznldbazie vbalzuws.',
    name: 'Taylor Davis',
    title: 'Professor',
    avatarUrl: 'https://i.pravatar.cc/150?img=4',
    rating: 5
  },
  {
    quote: 'Excellent service and amazing support team! The product exceeded my expectations in every way possible.',
    name: 'Sarah Wilson',
    title: 'CEO',
    avatarUrl: 'https://i.pravatar.cc/150?img=5',
    rating: 5
  },
  {
    quote: 'Outstanding quality and professional approach. Highly recommend this service to anyone looking for excellence.',
    name: 'Michael Brown',
    title: 'Director',
    avatarUrl: 'https://i.pravatar.cc/150?img=6',
    rating: 5
  },
  {
    quote: 'The best decision we made this year. The team is responsive and the results speak for themselves.',
    name: 'Emily Chen',
    title: 'Manager',
    avatarUrl: 'https://i.pravatar.cc/150?img=7',
    rating: 5
  },
  {
    quote: 'Top-notch service with attention to detail. Could not be happier with the outcome and support received.',
    name: 'David Martinez',
    title: 'Founder',
    avatarUrl: 'https://i.pravatar.cc/150?img=8',
    rating: 5
  }
]


//当前显示的起始索引 用于轮播切换
const currentStartIndex = ref(0)

//切换方向：'slide-left' 表示向左滑动，'slide-right' 表示向右滑动
const slideDirection = ref('slide-left')

//是否正在切换动画中（用于控制容器溢出）
const isAnimating = ref(false)

 //每次显示的卡片数量
const cardsPerPage = 4

/**
 * 计算当前应该显示的4张卡片
 * 使用循环索引，当到达数组末尾时自动回到开头
 */
const displayedTestimonials = computed(() => {
  const result = []
  for (let i = 0; i < cardsPerPage; i++) {
    const index = (currentStartIndex.value + i) % allTestimonials.length
    result.push(allTestimonials[index])
  }
  return result
})

/**
 * 导航到上一组卡片
 * 向前移动4个位置，支持循环
 * 设置动画方向为向右滑动，新卡片从左侧滑入
 */
const handlePrev = () => {
  slideDirection.value = 'slide-right'
  isAnimating.value = true // 开始动画
  currentStartIndex.value = (currentStartIndex.value - cardsPerPage + allTestimonials.length) % allTestimonials.length
  // 动画持续时间是 0.6s，在动画结束后恢复
  setTimeout(() => {
    isAnimating.value = false
  }, 600)
}

/**
 * 导航到下一组卡片
 * 向后移动4个位置，支持循环
 * 设置动画方向为向左滑动，新卡片从右侧滑入
 */
const handleNext = () => {
  slideDirection.value = 'slide-left'
  isAnimating.value = true // 开始动画
  currentStartIndex.value = (currentStartIndex.value + cardsPerPage) % allTestimonials.length
  // 动画持续时间是 0.6s，在动画结束后恢复
  setTimeout(() => {
    isAnimating.value = false
  }, 600)
}

</script>

<style lang="scss" scoped>
/* 主容器：居中布局，限制最大宽度 */
.testimonials-container {
  padding: 28px 14px;
  max-width: 1013px;
  margin: 0 auto;

  /* 头部区域：水平布局，包含副标题、主标题和导航按钮 */
  .header-section {
    position: relative;
    margin-bottom: 42px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    gap: 21px;

    /* 左上角副标题样式 */
    .subtitle {
      font-size: 14px;
      font-weight: 400;
      color: #000000;
      line-height: 1.5em;
      flex-shrink: 0;
    }

    /* 主标题容器：居中显示，占据中间空间 */
    .title-wrapper {
      text-align: center;
      flex: 1;
      
      /* 主标题样式 */
      .main-title {
        font-size: 50px;
        font-weight: 540;
        line-height: 1.5em;
        color: #000000;
        margin: 0;

        /* 渐变文本效果（当前使用纯色，渐变效果已注释） */
        .gradient-text {
          //background: linear-gradient(90deg, #6DB8FF 0%, #D09DFF 100%);
          //-webkit-background-clip: text;
          //-webkit-text-fill-color: transparent;
          //background-clip: text;
          color:#6DB8FF;
        }
      }
    }

    /* 导航按钮组：右侧对齐 */
    .navigation-buttons {
      display: flex;
      gap: 13px;
      flex-shrink: 0;

      /* 导航按钮基础样式 */
      .nav-btn {
        width: 42px;
        height: 42px;
        border: none;
        border-radius: 14px;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: transform 0.2s ease, opacity 0.2s ease;
        color: #1D1B20;

        /* 悬停效果 */
        &:hover {
          transform: scale(1.05);
          opacity: 0.9;
        }

        /* 点击效果 */
        &:active {
          transform: scale(0.95);
        }

        /* 上一个按钮：浅蓝色背景 */
        &.prev-btn {
          background: #d6ecfe;
        }

        /* 下一个按钮：渐变背景（蓝色到紫色） */
        &.next-btn {
          background: linear-gradient(90deg, rgb(161, 210, 255) 0%, rgb(218, 182, 255) 100%);
        }

        /* SVG 图标尺寸 */
        svg {
          width: 17px;
          height: 17px;
        }
      }
    }
  }

  /* 评价卡片网格容器 */
  .testimonials-grid {
    padding: 0px 14px 13px 14px; // 底部padding为阴影留出空间（3px阴影 + 10px安全距离）
    position: relative;
    min-height: 500px; // 防止切换时高度跳动
    overflow: visible; // 允许阴影和边框显示
    
    /* 动画期间只隐藏水平方向溢出，防止页面出现滚动条，但保持垂直滚动条 */
    &.is-animating {
      overflow-x: hidden; // 只隐藏水平方向的溢出
      overflow-y: visible; // 保持垂直方向可见，不影响页面滚动条
    }

    /* 内部网格容器 */
    .testimonials-grid-inner {
      display: grid;
      grid-template-columns: repeat(2, 1fr); // 桌面端：2列布局
      grid-template-rows: repeat(2, auto); // 固定2行，防止动画时布局混乱
      gap: 21px;
      position: relative; // 确保过渡动画正常工作
      padding: 10px 13px 13px 13px; // 左右和底部为阴影留出空间（3px阴影）
      min-height: calc(244px * 2 + 21px + 20px); // 固定最小高度：2行卡片高度 + gap + padding，防止动画时高度变化
    }

    /* 单个评价卡片样式 */
    .testimonial-card {
      background: #ffffff;
      border-radius: 14px;
      padding: 21px;
      position: relative;
      box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.25); // 卡片阴影效果
      border: 1px solid #D9D9D9;
      min-height: 244px;
      display: flex;
      flex-direction: column;
      
      /* 在动画过程中使用绝对定位，避免grid重新布局导致重叠 */
      &.slide-left-enter-active,
      &.slide-left-leave-active,
      &.slide-right-enter-active,
      &.slide-right-leave-active {
        position: absolute !important;
        // 宽度计算：50%容器宽度 - gap的一半（10.5px）- 右padding（13px）
        // 这样确保不会超出容器右边界
        width: calc(50% - 10.5px - 13px);
        z-index: 10; // 确保动画卡片在上层
        box-sizing: border-box; // 确保padding和border包含在宽度内
      }
      
      /* 根据data-card-index计算每个卡片在grid中的位置 */
      /* 第一行第一列 (index 0) */
      &[data-card-index="0"] {
        &.slide-left-enter-active,
        &.slide-left-leave-active,
        &.slide-right-enter-active,
        &.slide-right-leave-active {
          left: 13px; // 左padding
          top: 10px; // 上padding
        }
      }
      
      /* 第一行第二列 (index 1) */
      &[data-card-index="1"] {
        &.slide-left-enter-active,
        &.slide-left-leave-active,
        &.slide-right-enter-active,
        &.slide-right-leave-active {
          // 50%位置 + gap的一半（10.5px）
          left: calc(50% + 10.5px);
          top: 10px;
        }
      }
      
      /* 第二行第一列 (index 2) */
      &[data-card-index="2"] {
        &.slide-left-enter-active,
        &.slide-left-leave-active,
        &.slide-right-enter-active,
        &.slide-right-leave-active {
          left: 13px;
          top: calc(244px + 21px + 10px); // 卡片高度 + gap + 上padding
        }
      }
      
      /* 第二行第二列 (index 3) */
      &[data-card-index="3"] {
        &.slide-left-enter-active,
        &.slide-left-leave-active,
        &.slide-right-enter-active,
        &.slide-right-leave-active {
          left: calc(50% + 10.5px);
          top: calc(244px + 21px + 10px);
        }
      }

      /* 装饰性引用标记：左上角大引号 */
      .quotation-mark {
        position: absolute;
        top: 37px;
        left: 21px;
        font-size: 90px;
        font-weight: 290;
        line-height: 1;
        color: #6DB8FF;
        opacity: 0.4;
        transform: translate(-7px, -14px);
        z-index: 0; // 置于底层
        font-family: 'Microsoft Sans Serif';
      }

      /* 客户评价内容文本 */
      .testimonial-quote {
        font-size: 17px;
        font-weight: 400;
        line-height: 1.5em;
        color: #000000;
        margin: 42px 0 14px 0;
        flex: 1; // 占据剩余空间
        position: relative;
        z-index: 1; // 置于引用标记之上
      }

      /* 客户信息区域：底部对齐 */
      .customer-info {
        display: flex;
        align-items: center;
        gap: 14px;
        margin-top: auto; // 推到底部
        position: relative;
        z-index: 1;

        /* 客户头像容器 */
        .avatar-wrapper {
          .avatar {
            width: 47px;
            height: 47px;
            border-radius: 50%; // 圆形头像
            border: 1px solid #6DB8FF;
            object-fit: cover;
            background: linear-gradient(135deg, #6DB8FF 0%, #D09DFF 100%); // 头像占位背景
          }
        }

        /* 客户详细信息容器 */
        .customer-details {
          flex: 1;

          /* 第一行：姓名和评分水平排列 */
          .name-rating-row {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 5px;
          }

          /* 客户姓名样式 */
          .customer-name {
            font-size: 17px;
            font-weight: 400;
            line-height: 1.5em;
            color: #000000;
            margin: 0;
          }

          /* 客户职业/职位样式 */
          .customer-title {
            font-size: 14px;
            font-weight: 290;
            line-height: 1.5em;
            color: #000000;
            margin: 0;
          }

          /* 星级评分容器 */
          .rating {
            display: flex;
            gap: 2px;

            /* 单个星星样式 */
            .star {
              font-size: 14px;
              color: #d3d3d3; // 未填充星星颜色（灰色）
              line-height: 1;

              /* 填充的星星颜色（黄色） */
              &.filled {
                color: #FFEE00;
              }
            }
          }
        }
      }

      /* 偶数卡片样式变化：引用标记和头像边框使用紫色 */
      &:nth-child(2n) {
        .quotation-mark {
          color: #c892fb;
        }

        .customer-info {
          .avatar-wrapper {
            .avatar {
              border-color: #D09DFF;
            }
          }
        }
      }
    }
  }
}

/* 卡片切换过渡动画 - 向左滑动（下一个按钮） */
.slide-left-enter-active,
.slide-left-leave-active {
  transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.slide-left-enter-from {
  opacity: 0;
  transform: translateX(100%);
}

.slide-left-leave-to {
  opacity: 0;
  transform: translateX(-100%);
}

.slide-left-enter-to,
.slide-left-leave-from {
  opacity: 1;
  transform: translateX(0);
}

/* 卡片切换过渡动画 - 向右滑动（上一个按钮） */
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.slide-right-enter-from {
  opacity: 0;
  transform: translateX(-100%);
}

.slide-right-leave-to {
  opacity: 0;
  transform: translateX(100%);
}

.slide-right-enter-to,
.slide-right-leave-from {
  opacity: 1;
  transform: translateX(0);
}
</style>

