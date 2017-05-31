<template>
  <div class="hello">
    <h1>{{ thisdata.name }}</h1>
    <h2>{{thisdata.introduce}}</h2>
    <ul>
        <Darali v-for="(item,index) in thisdata.questions" :thisdata="item" :key="index" @childval="pushdata"/>
    </ul>
    <button @click="getdata">点我点我</button>
  </div>
</template>

<script>
import Darali from './datali'

export default {
  name: 'hello',
  components: {
    Darali
  },
  data () {
    return {
      thisdata: {
        name: '前段笔试题',
        introduce: '布姆电竞主要业务包括布姆视频，布姆电竞学院，教学内容覆盖英雄联盟，守望先锋，王者荣耀等当下热门的电竞游戏。',
        questions: [
          {type: 'text', question: '常用那几种浏览器测试？有哪些内核(Layout Engine)?'},
          {
            type: 'singleSelection',
            question: 'DOM怎样添加节点?',
            answers: [{answer: 'appendChild'}, {answer: 'getElementsByTagName'}, {answer: 'createTextNode'}, {answer: 'createDocumentFragment'}]
          }, {
            type: 'MultipleSelection',
            question: 'null和undefined的区别?',
            answers: [{answer: '变量被声明了，但没有赋值时，就等于undefined。'}, {answer: '调用函数时，应该提供的参数没有提供，该参数等于undefined。'}, {answer: ' 作为函数的参数，表示该函数的参数不是对象。'}, {answer: '创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。'}]
          }, {
            type: 'custom',
            question: 'null和undefined的区别?',
            hasAnswers: false,
            answers: [{type: 'text', question: '常用那几种浏览器测试？有哪些内核(Layout Engine)?'}, {
              type: 'MultipleSelection',
              question: 'null和undefined的区别?',
              answers: [{answer: '变量被声明了，但没有赋值时，就等于undefined。'}, {answer: '调用函数时，应该提供的参数没有提供，该参数等于undefined。'}, {answer: ' 作为函数的参数，表示该函数的参数不是对象。'}, {answer: '创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。'}]
            }]
          }
        ]
      },
      answers: []
    }
  },
  methods: {
    pushdata (type, question, answers) {
      console.log('收到' + type, question, answers)
      console.log(type === 'custom')
      this.answers.map((obj) => {
        if (type === 'custom' && obj.type === type) {
          if (obj.child) {
            obj.child.map((thisobj) => {
              console.log(thisobj, question)
              if (thisobj.question === question) {
                thisobj.answers = answers
              }
            })
          }
        } else {
          if (obj.type === type && obj.question === question) {
            obj.answers = answers
            console.log('不是custom')
          }
        }
      })
    },
    getdata () {
      console.log(this.answers)
    }
  },
  mounted () {
    const that = this
    this.thisdata.questions.map((obj) => {
      if (obj.type !== 'custom') {
        that.answers.push({question: obj.question, type: obj.type})
      } else {
        let child = []
        if (obj.hasAnswers === false) {
          obj.answers.map((cobj) => {
            child.push({question: cobj.question, type: cobj.type})
          })
        }
        that.answers.push({question: obj.question, type: obj.type, child: child})
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 20px 10px;
}

a {
  color: #42b983;
}
</style>
