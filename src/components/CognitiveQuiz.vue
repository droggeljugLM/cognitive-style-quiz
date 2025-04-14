<template>
    <div class="container">
      <h1>认知风格测试问卷</h1>
      <div v-if="current < questions.length" class="question-block">
        <h3>第 {{ current + 1 }} 题</h3>
        <p>{{ questions[current].text }}</p>
        <div class="options">
          <button
            v-for="(option, index) in questions[current].options"
            :key="index"
            @click="answer(index)"
          >
            {{ option }}
          </button>
        </div>
      </div>
      <div v-else class="result-block">
        <h2>测试完成！</h2>
        <h3>你的认知倾向得分：</h3>
        <p v-for="(style, index) in styles" :key="index">
          {{ style }}：{{ score[index] }}分
        </p>
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
        ]
      };
    },
    methods: {
      answer(index) {
        this.score[index]++;
        this.current++;
      }
    }
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 600px;
    margin: auto;
    padding: 2rem;
    background: #f9f9f9;
    color: #333;
    font-family: sans-serif;
  }
  .question-block, .result-block {
    margin-bottom: 2rem;
  }
  .options button {
    display: block;
    margin: 0.5rem 0;
    padding: 0.75rem;
    border: none;
    background-color: #e0e0e0;
    border-radius: 6px;
    cursor: pointer;
    width: 100%;
    font-size: 1rem;
  }
  .options button:hover {
    background-color: #ccc;
  }
  </style>
  