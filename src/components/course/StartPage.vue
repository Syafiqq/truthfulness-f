<template lang="pug">
  b-row
    b-col(md='12')
      b-card
        .form-group(v-for='option in cOptions')
          .pretty.p-default.p-smooth.font-xl
            input.form-control(type='radio', :value='option.id' v-model='answer')
            .state.p-info
              label {{option.description}}
        b-progress.mb-2(height='1.4 rem', :max='100')
          b-progress-bar(:value='cProgress', variant='info', striped='', :animated='true')
            strong.text-dark {{cProgress.toFixed(2)}} %
        h3.mb-0(slot='header')
          strong {{cQuestion}}
        div.center-text-align(slot='footer')
          button.align-content-center.btn.btn-sm.float-left.px-4(:class="cPrevAvailable ? 'btn-primary' : 'btn-danger'" type='button', @click='doPrev' :disabled='!cPrevAvailable') Sebelumnya
          button.align-content-center.btn.btn-primary.px-4(type='button', @click='doAnswer' :disabled='isDisabled') Jawab
          button.align-content-center.btn.btn-sm.float-right.px-4(:class="cNextAvailable ? 'btn-primary' : 'btn-danger'" type='button', @click='doNext' :disabled='!cNextAvailable') Selanjutnya
</template>

<script>

export default {
  name: 'CourseStartPage',
  props: {
    course: Object
  },
  data () {
    return {
      answer: null,
      disabled: false
    }
  },
  methods: {
    doAnswer () {

    },
    doNext () {
      this.$router.push({
        name: 'course-start',
        params: { id: this.$props?.course?.next?.order || this.$router.currentRoute.params.id }
      })
    },
    doPrev () {
      this.$router.push({
        name: 'course-start',
        params: { id: this.$props?.course?.prev?.order || this.$router.currentRoute.params.id }
      })
    }
  },
  watch: {
    course: function (val) {
      this.answer = val?.current?.answer || null
    }
  },
  computed: {
    isDisabled: function () {
      return this.disabled
    },
    cQuestion () {
      return this.$props.course.question?.question || ''
    },
    cOptions () {
      return this.$props.course.options || []
    },
    cNextAvailable () {
      return this.$props.course.next != null
    },
    cPrevAvailable () {
      return this.$props.course.prev != null
    },
    cProgress () {
      return this.$props.course.summary || 0
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>