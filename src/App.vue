<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <select v-model="alertAnimation" class="form-control">
                  <option value="fade">Fade</option>
                  <option value="slide">Slide</option>
                </select>
                <br></br>
                <button class="btn btn-primary" @click="show = !show">show alert</button>
                <br></br>
                <transition appear :name="alertAnimation">
                  <div class="alert alert-info" v-show="show">This is info</div>
                </transition>
                <!-- Mixing transition and animation -->
                <transition appear name="slide" type="animation">
                  <div class="alert alert-info" v-if="show">This is info</div>
                </transition>
                <!-- initial animation appear -->
                <transition
                  appear
                  enter-active-class="animated bounce"
                  leave-active-class="animated jello">
                  <div class="alert alert-info" v-if="show">This is info</div>
                </transition>
                <transition :name="alertAnimation" mode="out-in">
                  <!-- Add key property -->
                  <div class="alert alert-info" v-if="show" key="info">This is info</div>
                  <div class="alert alert-warning" v-else key="warning">This is Warning</div>
                </transition>
                <hr>
                <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
                <br><br>
                <transition
                  @before-enter="beforeEnter"
                  @enter="enter"
                  @after-enter="afterEnter"
                  @enter-cancelled="enterCancelled"

                  @before-leave="beforeLeave"
                  @leave="leave"
                  @after-leave="afterLeave"
                  @leave-cancelled="leaveCancelled"
                  :css="false">
                  <div style="width: 300px ; height: 100px; background-color:lightgreen" v-if="load"></div>
                </transition>
                <hr>
                <button class="btn btn-primary"
                  @click="selectedComponent == 'app-success-alert' ? selectedComponent = 'app-danger-alert' : selectedComponent = 'app-success-alert'"
                  >Toggle Components</button>
                <br><br>
                <transition name="fade" mode="out-in">
                    <component :is="selectedComponent"></component>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    import DangerAlert from './DangerAlert.vue'
    import SuccessAlert from './SuccessAlert.vue'

    export default {
        data() {
            return {
              show: false,
              load: true,
              alertAnimation: "fade",
              elementWidth: 100,
              selectedComponent: 'app-success-alert'
            }
        },
        methods: {
          beforeEnter(el) {
            console.log('beforEnter')
            this.elementWidth = 100
            el.style.width = this.elementWidth + 'px'
          },
          enter(el, done) {
            console.log('enter')
            let round = 1
            const interval = setInterval(() => {
              el.style.width = (this.elementWidth + round * 10) + 'px'
              round ++
              if(round > 20) {
                clearInterval(interval)
                done()
              }
            }, 20)
            done()
          },
          afterEnter(el) {
            console.log('afterenter')
          },
          enterCancelled(el) {
            console.log('enterCancelled')
          },
          beforeLeave(el) {
            console.log('beforeLeave')
            this.elementWidth = 300
            el.style.width = this.elementWidth + 'px'
          },
          leave(el, done) {
            console.log('leave')
            let round = 1
            const interval = setInterval(() => {
              el.style.width = (this.elementWidth - round * 10) + 'px'
              round ++
              if(round > 20) {
                clearInterval(interval)
                done()
              }
            }, 20)
          },
          afterLeave(el) {
            console.log('afterLeave')
          },
          leaveCancelled(el) {
            console.log('leaveCancelled')
          }
        },
        components: {
          appDangerAlert: DangerAlert,
          appSuccessAlert: SuccessAlert
        }
    }
</script>

<style>
  .fade-enter{
    opacity: 0;
  }

  .fade-enter-active{
    transition: opacity 1s;
  }

  .fade-leave{
    /*opacity: 1;*/
  }

  .fade-leave-active{
    transition: opacity 1s;
    opacity: 0;
  }

  .slide-enter{
    opacity: 0;
    /*transform: translateY(20px);*/
  }

  .slide-enter-active{
    animation: slide-in 1s ease-out forwards;
    transition: opacity .5s;
  }

  .slide-leave{

  }

  .slide-leave-active{
    animation: slide-out 1s ease-out forwards;
    transition: opacity 1s;
    opacity: 0;
  }

  @keyframes slide-in {
    from {
      transform: translateY(20px);
    }
    to {
      transform: translateY(0);
    }
  }

  @keyframes slide-out {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(20px);
    }
  }
</style>
