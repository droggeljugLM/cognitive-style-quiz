<template>
    <div class="container">
      <h1>认知风格测试问卷</h1>
  
      <!-- 进度条 -->
      <div class="progress-bar">
        <div class="progress" :style="{ width: progressPercent + '%' }"></div>
      </div>
  
      <div v-if="current < questions.length" class="question-block">
        <h3>第 {{ current + 1 }} 题</h3>
        <p>{{ questions[current].text }}</p>
        <div class="options">
          <button
            v-for="(option, index) in shuffledOptions"
            :key="index"
            @click="answer(option.originalIndex)"
          >
            {{ option.text }}
          </button>
        </div>
      </div>
  
      <div v-else class="result-block">
        <h2>测试完成！</h2>
        <h3>你的认知倾向得分：</h3>
  
        <!-- 分数图表 -->
        <div class="bar-chart">
          <div
            v-for="(style, index) in styles"
            :key="index"
            class="bar-item"
          >
            <span>{{ style }}</span>
            <div class="bar">
              <div
                class="bar-fill"
                :style="{ width: (score[index] / totalQuestions * 100) + '%' }"
              ></div>
              <span class="score">{{ score[index] }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        current: 0,
        score: [0, 0, 0, 0],
        styles: ["直觉主义", "逻辑主义", "理性主义", "意志主义"],
        questions: [
          {
            text: "深夜写作时，你突然感觉某句话“非常有道理”，但说不清楚为什么。你会？",
            options: [
              "尝试从整体感觉出发继续写下去",
              "分析这句话的推理过程是否严密",
              "回想这句话背后的因果或现实依据",
              "保留这句话不管原因，因为你觉得它很有力量"
            ]
          }
          // TODO: 添加第 2~20 题
        ],
        shuffledOptions: []
      };
    },
    computed: {
      totalQuestions() {
        return this.questions.length;
      },
      progressPercent() {
        return Math.round((this.current / this.totalQuestions) * 100);
      }
    },
    created() {
      this.shuffleCurrentOptions();
    },
    methods: {
      shuffleCurrentOptions() {
        const currentOptions = this.questions[this.current].options;
        this.shuffledOptions = currentOptions
          .map((text, index) => ({ text, originalIndex: index }))
          .sort(() => Math.random() - 0.5);
      },
      answer(index) {
        this.score[index]++;
        this.current++;
        if (this.current < this.questions.length) {
          this.shuffleCurrentOptions();
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 600px;
    margin: auto;
    padding: 2rem;
    background: #ffffff;
    color: #1a1a1a;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  }
  .question-block, .result-block {
    margin-bottom: 2rem;
  }
  .options button {
    display: block;
    margin: 0.5rem 0;
    padding: 0.75rem;
    border: 1px solid #cccccc;
    background-color: #ffffff;
    border-radius: 8px;
    cursor: pointer;
    width: 100%;
    font-size: 1rem;
    transition: all 0.2s ease;
    color: #333;
  }
  .options button:hover {
    background-color: #007bff;
    color: #fff;
    border-color: #007bff;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.3);
  }
  .progress-bar {
    width: 100%;
    height: 8px;
    background: #eee;
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 1rem;
  }
  .progress {
    height: 100%;
    background: #007bff;
    transition: width 0.3s ease;
  }
  .bar-chart {
    margin-top: 1rem;
  }
  .bar-item {
    margin-bottom: 1rem;
  }
  .bar {
    display: flex;
    align-items: center;
    background: #f0f0f0;
    border-radius: 6px;
    overflow: hidden;
    height: 24px;
  }
  .bar-fill {
    height: 100%;
    background: #007bff;
    transition: width 0.3s ease;
  }
  .score {
    margin-left: 8px;
    padding-left: 8px;
    white-space: nowrap;
    font-size: 0.9rem;
  }
  </style>
  