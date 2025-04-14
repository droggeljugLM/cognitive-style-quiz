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
                <button v-for="(option, index) in shuffledOptions" :key="index" @click="answer(option.originalIndex)">
                    {{ option.text }}
                </button>
            </div>
        </div>

        <div v-else class="result-block">
            <h2>测试完成！</h2>
            <h3>你的认知倾向得分：</h3>

            <!-- 分数图表 -->
            <div class="bar-chart">
                <div v-for="(style, index) in styles" :key="index" class="bar-item">
                    <span>{{ style }}</span>
                    <div class="bar">
                        <div class="bar-fill" :style="{ width: (score[index] / totalQuestions * 100) + '%' }"></div>
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
</style>