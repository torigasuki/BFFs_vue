<template>
    <div class="alarm-main">
        <div class="alarm-currentplaying">
            <p class="alarm-heading">알림 목록</p>
        </div>
        <div class="alarm-songs">
            <div class="alarm-loader" v-for="alarm, index in alarms" :key="index">
                <div class="alarm-song">
                    <div class="alarm-song-name">
                        <router-link :to="`/community/detail/${alarm.community_name}/feed/${alarm.feed}`">
                            <p class="alarm-name" style="font-size: 14px; margin-bottom: 0px; color: #909090;">{{ alarm.created_at }}</p>
                            <p class="alarm-name">{{ alarm.message }}</p>
                        </router-link>
                        <div @click="alarmDelete(alarm.id,index)">
                            <font-awesome-icon :icon="['fas', 'xmark']" style="color: black;" class="icon"/>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="alarm-currentplaying2" @click="allalarmDelete()">
            <p class="alarm-heading2">모두 지우기</p>
        </div>   
    </div>
</template>

<script>
import { mapGetters } from 'vuex';
import { fetchAlarmDelete, fetchAllAlarmDelete } from "@/api/index.js";
import bus from '@/utils/bus';
export default {
    computed: {
        ...mapGetters(['getAlarmData']),
        alarms(){
            return this.getAlarmData;
        }
    },
    methods: {
        async alarmDelete(alarm_id,index) {
            try {
                const response = await fetchAlarmDelete(alarm_id);
                    if (response.status === 200) {
                        this.$store.dispatch('deleteAlarm', index);
                        this.snotify('success',response.data.message);
                    }
            } catch (error) {
                    this.snotify('error','삭제를 실패했습니다');
            }
        },
        async allalarmDelete() {
            try {
                const response = await fetchAllAlarmDelete();
                    if (response.status === 200) {
                        this.$store.dispatch('deleteAllAlarm');
                        this.snotify('success',response.data.message);
                    }
            } catch (error) {
                    this.snotify('error','삭제를 실패했습니다');
            }
        },
        snotify(type, message) {
            bus.$emit('showSnackbar', {
                type,
                message
            });
        },
    },
}

</script>

<style scope>
a {
    text-decoration: none;
}
.alarm-main {
  background-color: white;
  padding: 1em;
  padding-bottom: 1.1em;
  border-radius: 15px;
  /*margin: 1em;*/

  width: 300px;
  position: absolute;
  z-index: 100;
  top: 120px;
  right: 0px;
  margin: 0 auto;
  border: 1px solid #e7eae8;
}

.alarm-songs {
display: flex;
flex-direction: column;
gap: .5rem;
height: 210px;
overflow-y: auto;
}
.alarm-songs::-webkit-scrollbar {
width: 5px;
}
.alarm-songs::-webkit-scrollbar-track {
background: #f1f1f1;
}
.alarm-songs::-webkit-scrollbar-thumb {
background: #888;
}
.alarm-song-name {
  display: flex; 
  align-items: center;
  justify-content: space-between;
  height: 70px;
}
.alarm-loader {
  display: flex;
  flex-direction: row;
  height: 4em;
  padding-left: 1em;
  padding-right: 1em;
  transform: rotate(180deg);
  justify-content: right;
  border-radius: 10px;
  transition: .4s ease-in-out;
}

.alarm-loader:hover {
  cursor: pointer;
  background-color: lightgray;
}

.alarm-currentplaying {
  display: flex;
  align-self: center;
width: 100%;
border-radius: .5rem;
}
.alarm-currentplaying2 {
  display: flex;
  align-self: center;
width: 100%;
border-radius: .5rem;

}

.alarm-spotify {
  width: 50px;
  height: 50px;
  margin-right: 0.6em;
}

.alarm-heading {
    letter-spacing: 10px;
    color: #555555;
font-size: 1.1em;
font-weight: bold;
align-self: center;
margin: 0 auto;
margin-top: 18px;
margin-bottom: 18px;
}
.alarm-heading2 {
    letter-spacing: 2px;
    color: #909090;
font-size: 14px;
font-weight: bold;
align-self: center;
margin: 0 auto;
margin-top: 10px;
cursor: pointer;
}

.alarm-load {
  width: 2px;
  height: 33px;
  background-color: limegreen;
  animation: 1s move6 infinite;
  border-radius: 5px;
  margin: 0.1em;
}

.alarm-load:nth-child(1) {
  animation-delay: 0.2s;
}

.alarm-load:nth-child(2) {
  animation-delay: 0.4s;
}

.alarm-load:nth-child(3) {
  animation-delay: 0.6s;
}

.alarm-song {
  position: relative;
  transform: rotate(180deg);
  color: black;
  align-self: center;
  width: 100%;
}
.alarm-name {
    margin-top: 0px;
    color: black;
}

@keyframes move6 {
  0% {
    height: 0.2em;
  }

  25% {
    height: 0.7em;
  }

  50% {
    height: 1.5em;
  }

  100% {
    height: 0.2em;
  }
}
</style>