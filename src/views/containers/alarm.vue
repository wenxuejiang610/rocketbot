<template>
<div>
  <alarm-nav>
    <a class="rk-alarm-nav-i" @click="type='APPLICATION'" :class="{'active':type === 'APPLICATION'}">Application</a>
    <a class="rk-alarm-nav-i" @click="type='SERVER'" :class="{'active':type === 'SERVER'}">Server</a>
    <a class="rk-alarm-nav-i" @click="type='SERVICE'" :class="{'active':type === 'SERVICE'}">Service</a>
  </alarm-nav>
  <div class="rk-alarm">
    <table class="rk-alarm-table">
      <thead>
        <tr><th>&nbsp;</th><th>Start</th><th>Title</th><th>Type</th><th>Content</th></tr>
      </thead>
      <tbody>
         <tr v-for="(i, index) in stateAlarm.alarmList" :key="index">
          <td><ion-icon name="information-circle" class="rk-alarm-icon"></ion-icon></td>
          <td class="grey ell">{{i.startTime}}</td>
          <td>{{i.title}}</td>
          <td>{{i.causeType}}</td>
          <td>{{i.content}}</td>
        </tr>
      </tbody>
    </table>
    <RkEmpty text="no alarm" v-if="!stateAlarm.alarmList.length"/>
  </div>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import { State, Action } from 'vuex-class';
import { Component, Watch } from 'vue-property-decorator';
import { getAlarm } from '@/store/dispatch/alarm.ts';
import AlarmItem from '../components/alarm/alarm-item.vue';
import AlarmNav from '../components/alarm/alarm-nav.vue';

@Component({
  components: { AlarmItem, AlarmNav },
})
export default class Alarm extends Vue {
  @State('alarm') stateAlarm;
  @Action('SET_EVENTS') SET_EVENTS;
  page:Number = 1;
  type:String = 'APPLICATION';
  @Watch('type')
  onTypeChanged() {
    this.getAlarm();
  }
  beforeMount() {
    this.getAlarm();
    this.SET_EVENTS([this.getAlarm]);
  }
  getAlarm() {
    getAlarm({ type: this.type, paging: this.page });
  }
}
</script>

<style lang="scss">
.rk-alarm{
  padding: 15px 20px;
}
.rk-alarm-box{
  width: 100%;
  padding: 15px;
}
.rk-alarm-table{
  width: 100%;
  text-align: left;
  .rk-alarm-icon{
    font-size:19px;
    color:#f7b32b;
    line-height:1px;
    display:block;
    margin: 0 auto;
  }
  th{
    padding: 8px 10px;
    border-bottom: 1px solid #e4e7ed;
  }
  td{
    padding: 8px 10px;
    border-bottom: 1px solid #e4e7ed;
  }
}
</style>
