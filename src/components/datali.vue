<template>
  <li class="data">
    <p class="datatitle">{{thisdata.question}}</p>
    <br />
    <textarea v-if="thisdata.type==='text'" v-model="answers"></textarea>
    <p v-if="thisdata.type==='singleSelection'" v-for="item in thisdata.answers">
        <input :name="thisdata.question" type="radio" :value="item.answer"  v-model="answers"/> {{item.answer}} 
    </p>
    <p v-if="thisdata.type==='MultipleSelection'" v-for="item in thisdata.answers">
        <input type="checkbox" :id="item.answer" :value="item.answer" v-model="answers"> {{item.answer}} 
    </p>
    <div v-if="thisdata.type==='custom'">
        <div v-if="thisdata.hasAnswers==='hasAnswers'">
            {{thisdata.answers}}
        </div>
        <div else>
            <datali v-for="(item,index) in thisdata.answers" :thisdata="item" :key="index" @childval="toparval1"/>
        </div>
    </div>
  </li>
</template>

<script>

export default {
  name: 'datali',
  props: ['thisdata'],
  data () {
    return {
      answers: this.thisdata.type === 'MultipleSelection' ? [] : ''
    }
  },
  watch: {
    answers: function (val, old) {
      this.toparval(val)
    }
  },
  methods: {
    toparval (answers) {
      this.$emit('childval', this.thisdata.type, this.thisdata.question, answers)
    },
    toparval1 (type, question, answers) {
      this.$emit('childval', 'custom', question, answers)
    }
  },
  mounted () {
  }
}
</script>

<style>
textarea{
    height: 80px;
    width: 500px;
    margin: 10px;
}
.datatitle{
    color: #42b983;
}
</style>
