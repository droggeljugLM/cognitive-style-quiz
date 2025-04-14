<template>
  <div class="container fixed-width">
    <h1>认知风格测试问卷</h1>

    <div class="progress-bar">
      <div class="progress" :style="{ width: progressPercent + '%' }"></div>
    </div>
    <transition name="fade" mode="out-in">
      <div v-if="current < questions.length" :key="current" class="question-block">
        <h3>第 {{ current + 1 }} 题</h3>
        <p>{{ questions[current].text }}</p>
        <div class="options">
          <transition-group name="fade" tag="div">
            <button v-for="(option, index) in shuffledOptions" :key="index + '-' + current"
              @click="answer(option.originalIndex)">
              {{ option.text }}
            </button>
          </transition-group>
        </div>
      </div>

      <div v-else :key="'result'" class="result-block">
        <h2 class="celebrate">🎉 测试完成！</h2>
        <h3>你的认知倾向得分：</h3>
        <div class="bar-chart">
          <div v-for="(style, index) in styles" :key="index" class="bar-item">
            <span>{{ style }}</span>
            <div class="bar">
              <div class="bar-fill" :style="{ width: (score[index] / totalQuestions * 100) + '%' }"></div>
              <span class="score">{{ score[index] }}</span>
            </div>
          </div>
        </div>
        <div class="confetti"></div>
      </div>
    </transition>
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
        },
        {
          text: "你正在为朋友解决一个复杂的人际冲突。他向你求助：‘我该不该联系对方？’你会？",
          options: [
            "倾听自己的第一直觉并说出建议",
            "先分析每个选择带来的逻辑后果再判断",
            "探讨事情的前因后果，看看哪种选择更‘合理’",
            "鼓励朋友想清楚‘你到底想怎么做’，再做决定"
          ]
        },
        {
          text: "你在博物馆看到一幅抽象画，没人能说清它想表达什么。你会？",
          options: [
            "站在那里感受它传达出的某种情绪",
            "分析构图、比例和色彩之间是否有内在规律",
            "去查这位画家的背景和作品解释",
            "想象自己是这幅画的创作者，并思考‘我想说什么’"
          ]
        },
        {
          text: "你计划发起一个新的创作项目，大家各有看法。你倾向于：",
          options: [
            "听从自己内心的冲动，哪怕一时冲动也要试试",
            "规划一整套步骤、里程碑、预期路径",
            "先调查需求、可行性和历史案例再决定",
            "不管别人说什么，你就是想做这件事"
          ]
        },
        {
          text: "和朋友争论某个观点时，对方说‘你怎么知道你是对的？’你会？",
          options: [
            "我就是有种强烈的感觉",
            "因为这从逻辑推理上不可能错",
            "举出一连串事实或因果链证明合理性",
            "对错不重要，我认定这就是我要坚持的"
          ]
        },
        {
          text: "你参加一个辩论赛，被要求站在你不认同的一边，你会？",
          options: [
            "试图从感受上找到可以共情的角度",
            "靠逻辑规则来构建无懈可击的论证结构",
            "理解那一立场的原因与动机",
            "暂时压抑立场，只关注赢下这场比赛的目标"
          ]
        },
        {
          text: "面对一个模糊问题，比如‘幸福是什么？’，你会：",
          options: [
            "说：‘对我来说，幸福是一种感受’",
            "分析‘幸福’定义的逻辑构成与分类",
            "探讨心理学与哲学中幸福的不同理论和因果基础",
            "表达：‘我选择相信我自己的幸福定义’"
          ]
        },
        {
          text: "当你对一个项目感到迷茫，下一步该做什么？",
          options: [
            "等待内心涌现出一种‘对的感觉’",
            "画一个思维导图，梳理逻辑和行动路径",
            "找资料、请教专家、建立知识模型",
            "强迫自己先动起来，边做边找方向"
          ]
        },
        {
          text: "朋友想和你合写一本小说，但你们审美完全不同。你会？",
          options: [
            "尝试感受对方的风格，说不定能互补",
            "拿出对比列表，一项项判断谁更适合写哪部分",
            "分析读者需求与市场取向来决定风格融合方式",
            "坚持表达你最想写的部分，哪怕冒点风险"
          ]
        },
        {
          text: "如果你必须快速做出一个重要决定，你更可能？",
          options: [
            "跟随直觉做出选择",
            "快速列出利弊并判断逻辑上的最优选",
            "想清楚这件事的背景与后果再判断",
            "立刻拍板执行，不纠结"
          ]
        },
        {
          text: "你写完一篇文章后，别人说：‘你这个地方说不通。’你会？",
          options: [
            "我当时的感觉是这样，应该有它的意义",
            "检查句子结构、论证流程是否逻辑出错",
            "回顾上下文是否缺乏说明或证据",
            "坚持表达不改，因为它传达了我想表达的力量"
          ]
        },
        {
          text: "在一个集体讨论中，你是第一个被问‘你怎么看？’你会？",
          options: [
            "说出脑中闪现的第一感觉",
            "临时构造一个逻辑清晰的观点架构",
            "快速回忆类似话题的背景知识并给出合理建议",
            "表达你一直坚持的立场"
          ]
        },
        {
          text: "某人做出一个你无法理解的决定，你会？",
          options: [
            "也许他有种特别的感觉",
            "分析是否符合逻辑目标",
            "推测他的行为动机和因果链",
            "尊重他的意志：那是他选择的路"
          ]
        },
        {
          text: "你在旅行时迷路了，导航失效。你会？",
          options: [
            "凭感觉选一条路走走看",
            "分析地形、太阳方向、分叉结构再决策",
            "回忆来时路线与地图特征进行判断",
            "随便选一条先走，边走边找出路"
          ]
        },
        {
          text: "你正在面对一项失败的实验，导师问你‘你怎么想？’你会？",
          options: [
            "我感觉我们其实接近了正确方向",
            "从实验设计中找结构漏洞",
            "分析变量控制、原理假设与干扰因素",
            "表示还想继续做，不想就此放弃"
          ]
        },
        {
          text: "面对一个社会争议事件，你在网上看到不同意见。你会？",
          options: [
            "关注自己对每种观点的直觉反应",
            "判断论点中是否存在逻辑谬误",
            "深挖背景、数据与成因",
            "直接表达你相信什么，不管别人怎么说"
          ]
        },
        {
          text: "你开始一项新工作，方法理念和你原本完全不同。你会？",
          options: [
            "先适应看看是否感觉顺手",
            "梳理系统流程判断是否更优",
            "调查这种方法背后的发展背景",
            "决定坚持你自己的一套看看会怎样"
          ]
        },
        {
          text: "你想改变某个坏习惯，比如拖延。你最可能的做法是？",
          options: [
            "用今天的感觉判断是否该行动",
            "制定详细流程和目标拆解",
            "找出拖延背后的心理原因，解决根源",
            "强行开始，不管有没有准备好"
          ]
        },
        {
          text: "朋友问你‘你为什么总是相信自己的选择？’你会？",
          options: [
            "因为我的直觉从来没让我后悔过",
            "因为我把事情都推理清楚了",
            "因为我分析过各种可能性后得出这个判断",
            "因为这是我决定要走的路，不需要别人的认可"
          ]
        },
        {
          text: "在一次重要考试中，你遇到一道不会的题。你会？",
          options: [
            "凭感觉写一个你觉得‘可能对’的答案",
            "利用条件一步步推理哪怕不确定",
            "根据类似题型和知识点联想出一个合理答案",
            "随便写一个答案，跳过去集中精力完成其他题"
          ]
        }
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
      } else {
        setTimeout(() => {
          const confetti = document.querySelector(".confetti");
          if (confetti) confetti.classList.add("active");
        }, 400);
      }
    }
  }
};
</script>

<style scoped>
.container.fixed-width {
  width: 960px;
  max-width: 100%;
  margin: 2rem auto;
  padding: 2rem;
  background: #ffffff;
  color: #1a1a1a;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  border: 1px solid #ddd;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.05);
  border-radius: 8px;
  box-sizing: border-box;
}

.question-block,
.result-block {
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

@media (max-width: 768px) {
  .container.fixed-width {
    padding: 1rem;
    width: 100%;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.confetti {
  pointer-events: none;
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-image: radial-gradient(circle, rgba(255, 0, 102, 0.2) 1px, transparent 1px),
    radial-gradient(circle, rgba(0, 204, 255, 0.2) 1px, transparent 1px);
  background-size: 12px 12px;
  opacity: 0;
  transition: opacity 0.6s ease;
  z-index: 10;
}

.confetti.active {
  opacity: 1;
  animation: confetti-fadeout 2.5s forwards;
}

@keyframes confetti-fadeout {
  0% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}
</style>