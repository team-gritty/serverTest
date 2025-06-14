<template>
  <div>
    <apexchart type="bar" height="320" :options="chartOptions" :series="series" :key="chartKey" />
  </div>
</template>

<script setup>
// 필요한 vue 함수 import
import { ref, watch, computed, watchEffect } from 'vue'
import VueApexCharts from 'vue3-apexcharts'

// reactive한 차트 key를 computed로 정의
const chartKey = computed(() => `${props.channelId}-${myChannelLikeRatio.value}-${overallLikeRatio.value}`)

// 부모 컴포넌트에서 channels와 channelId를 props로 받음
const props = defineProps({
  channels: {
    type: Array,
    required: true
  },
  channelId: {
    type: [Number, String], // 숫자 또는 문자열 모두 허용
    required: true
  }
})

// props가 바뀔 때마다 콘솔에 확인 로그 찍기 (디버깅용)
watchEffect(() => {
  console.log('LikeRatioBarChart - channelId:', props.channelId)
  console.log('LikeRatioBarChart - channels:', props.channels)
})

// 내 채널의 좋아요/조회수 비율을 계산하는 computed
const myChannelLikeRatio = computed(() => {
   // 내 채널 찾기
  const my = (props.channels || []).find(c => String(c.id) === String(props.channelId))
  if (my) {
    // 내 채널의 비율 계산 (조회수가 0 초과일 때만)
    const ratio = my.avgViewCount > 0 ? my.avgLikeCount / my.avgViewCount : 0
    console.log('내 채널:', my)
    console.log('내 채널 비율:', ratio)
    return ratio
  }
  return 0
})

// 전체 채널의 평균 좋아요/조회수 비율 계산
const overallLikeRatio = computed(() => {
  // 조회수가 0 초과인 채널만 필터링
  const validChannels = (props.channels || []).filter(c => c.avgViewCount > 0)
  if (validChannels.length === 0) return 0
  // 총 좋아요/조회수 합계
  const totalLike = validChannels.reduce((sum, c) => sum + c.avgLikeCount, 0)
  const totalView = validChannels.reduce((sum, c) => sum + c.avgViewCount, 0)
   // 전체 평균 비율 계산
  return totalView > 0 ? totalLike / totalView : 0
})

// 차트 series 데이터 (항상 최신 computed 값 반영)
const series = computed(() => [
  {
    name: '좋아요 비율', // 데이터 라벨 
    data: [myChannelLikeRatio.value, overallLikeRatio.value]  // 내 채널과 전체 평균 비율
  }
])

// ApexCharts의 옵션을 computed로 정의
const chartOptions = computed(() => ({
  chart: { id: 'like-ratio-bar', toolbar: { show: false } }, // 차트 ID, 툴바 숨김
  xaxis: { categories: ['내 채널', '전체'] },                // X축 레이블
  yaxis: {
    min: 0,                                                 // Y축 최소값 0
    labels: { formatter: val => val.toFixed(2) }            // Y축 숫자 표시 형식
  },
  colors: ['#6B46C1', '#9F7AEA'],                          // 색상 배열
  dataLabels: { enabled: false },                          // 데이터 라벨 숨김
  grid: { borderColor: '#eee' },                           // 차트 격자선 색
  tooltip: {
    y: { formatter: val => (val * 100).toFixed(2) + '%' }  // 툴팁 숫자 % 형식
  },
  plotOptions: {
    bar: {
      borderRadius: 5,                                      // 막대 둥근 정도
      columnWidth: '40%',                                   // 막대 너비
      distributed: true                                     // 각 막대에 다른 색
    }
  }
}))
</script>

<script>
// apexchart 컴포넌트를 전역 등록
export default {
  components: {
    apexchart: VueApexCharts
  }
}
</script> 