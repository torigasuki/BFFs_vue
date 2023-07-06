<template>
    <div class="main">
        <div class="currentplaying">
            <p class="heading">알림 목록</p>
        </div>
        <div class="songs">
            <div class="loader" v-for="alarm, index in alarms" :key="index">
                <div class="song">
                    <div class="song-name">
                        <router-link :to="`/community/detail/${alarm.community_name}/feed/${alarm.feed}`">
                            <p class="name" style="font-size: 14px; margin-bottom: 0px; color: #909090;">{{ alarm.created_at }}</p>
                            <p class="name">{{ alarm.message }}</p>
                        </router-link>
                        <div @click="alarmDelete(alarm.id,index)">
                            <font-awesome-icon :icon="['fas', 'xmark']" style="color: black;" class="icon"/>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="currentplaying2" @click="allalarmDelete()">
            <p class="heading2">모두 지우기</p>
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

<style>
a {
    text-decoration: none;
}
.main {
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

.songs {
display: flex;
flex-direction: column;
gap: .5rem;
height: 210px;
overflow-y: auto;
}
.songs::-webkit-scrollbar {
width: 5px;
}
.songs::-webkit-scrollbar-track {
background: #f1f1f1;
}
.songs::-webkit-scrollbar-thumb {
background: #888;
}
.song-name {
  display: flex; 
  align-items: center;
  justify-content: space-between;
  height: 70px;
}
.loader {
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

.loader:hover {
  cursor: pointer;
  background-color: lightgray;
}

.currentplaying {
  display: flex;
  align-self: center;
width: 100%;
border-radius: .5rem;
}
.currentplaying2 {
  display: flex;
  align-self: center;
width: 100%;
border-radius: .5rem;

}

.spotify {
  width: 50px;
  height: 50px;
  margin-right: 0.6em;
}

.heading {
    letter-spacing: 10px;
    color: #555555;
font-size: 1.1em;
font-weight: bold;
align-self: center;
margin: 0 auto;
margin-top: 18px;
margin-bottom: 18px;
}
.heading2 {
    letter-spacing: 2px;
    color: #909090;
font-size: 14px;
font-weight: bold;
align-self: center;
margin: 0 auto;
margin-top: 10px;
cursor: pointer;
}

.loading {
  display: flex;
  margin-top: 1em;
  margin-left: 0.3em;
}

.load {
  width: 2px;
  height: 33px;
  background-color: limegreen;
  animation: 1s move6 infinite;
  border-radius: 5px;
  margin: 0.1em;
}

.load:nth-child(1) {
  animation-delay: 0.2s;
}

.load:nth-child(2) {
  animation-delay: 0.4s;
}

.load:nth-child(3) {
  animation-delay: 0.6s;
}

.play {
  position: relative;
  left: 0.35em;
  height: 1.6em;
  width: 1.6em;
  clip-path: polygon(50% 50%, 100% 50%, 75% 6.6%);
  background-color: black;
  transform: rotate(-90deg);
  align-self: center;
  margin-top: 0.7em;
  justify-self: center;
}

.albumcover {
  position: relative;
  margin-right: 1em;
  height: 40px;
  width: 40px;
  background-color: rgb(233, 232, 232);
  align-self: center;
  border-radius: 5px;
}

.song {
  position: relative;
  transform: rotate(180deg);
  color: black;
  align-self: center;
  width: 100%;
}
.name {
    margin-top: 0px;
    color: black;
}

.artist {
  font-size: 0.6em;
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