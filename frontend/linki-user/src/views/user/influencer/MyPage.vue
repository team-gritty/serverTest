<template>
  <div class="my-page">
    <MyPageSideBar v-model:currentMenu="currentMenu" />
      
    <div class="content-area">
      <!-- 내 정보 -->
      <MyPageBasicInfo v-if="currentMenu === 'profile.basic'" />
<!--      <MyPageChannelInfo v-if="currentMenu === 'profile.channel'" />-->
      <MyPagePassword v-if="currentMenu === 'profile.password'" />
      
      <!-- 캠페인 관리 -->
      <MyPageProposalList v-if="currentMenu === 'campaign.proposals'" />
      
      <!-- 리뷰 관리 -->
      <MyPageReceivedReviews v-if="currentMenu === 'review.received'" />
      <MyPageWrittenReviews v-if="currentMenu === 'review.written'" />
      <MyPageWriteReview v-if="currentMenu === 'review.write'" />
      
      <!-- 계약 관리 -->
      <MyPageOngoingContracts v-if="currentMenu === 'contract.ongoing'" />
      <MyPageCompletedContracts v-if="currentMenu === 'contract.completed'" />
      <MyPageSettlement v-if="currentMenu === 'contract.settlement'" />
        
      <!-- 구독 관리 -->
      <MyPageSubscriptionApply v-if="currentMenu === 'subscription.apply'" />
      <MyPageSubscriptionManage v-if="currentMenu === 'subscription.manage'" v-model:currentMenu="currentMenu" />
      <MyPageSubscriptionRefund v-if="currentMenu === 'subscription.refund'" />
    </div>
  </div>
</template>

<script>
import { ref, watch, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import MyPageSideBar from '@/components/user/influencer/mypage/MyPageSideBar.vue';
import MyPageProposalList from '@/components/user/influencer/mypage/MyPageProposalList.vue';
import MyPageBasicInfo from '@/components/user/influencer/mypage/MyPageBasicInfo.vue';
import MyPageChannelInfo from '@/components/user/influencer/mypage/MyPageChannelInfo.vue';
import MyPagePassword from '@/components/user/influencer/mypage/MyPagePassword.vue';
import MyPageReceivedReviews from '@/components/user/influencer/mypage/MyPageReceivedReviews.vue';
import MyPageWrittenReviews from '@/components/user/influencer/mypage/MyPageWrittenReviews.vue';
import MyPageWriteReview from '@/components/user/influencer/mypage/MyPageWriteReview.vue';
import MyPageOngoingContracts from '@/components/user/influencer/mypage/MyPageOngoingContracts.vue';
import MyPageCompletedContracts from '@/components/user/influencer/mypage/MyPageCompletedContracts.vue';
import MyPageSettlement from '@/components/user/influencer/mypage/MyPageSettlement.vue';
import MyPageSubscriptionApply from '@/components/user/influencer/mypage/MyPageSubscriptionApply.vue';
import MyPageSubscriptionManage from '@/components/user/influencer/mypage/MyPageSubscriptionManage.vue';
import MyPageSubscriptionRefund from '@/components/user/influencer/mypage/MyPageSubscriptionRefund.vue';

export default {
  name: 'MyPage',
  components: {
    MyPageSideBar,
    MyPageProposalList,
    MyPageBasicInfo,
    MyPageChannelInfo,
    MyPagePassword,
    MyPageReceivedReviews,
    MyPageWrittenReviews,
    MyPageWriteReview,
    MyPageOngoingContracts,
    MyPageCompletedContracts,
    MyPageSettlement,
    MyPageSubscriptionApply,
    MyPageSubscriptionManage,
    MyPageSubscriptionRefund
  },
  
  setup() {
    const route = useRoute();
    const currentMenu = ref('profile.basic');
    
    const updateMenuFromQuery = () => {
      const queryMenu = route.query.currentMenu;
      if (queryMenu) {
        currentMenu.value = queryMenu.toString();
      }
    };

    onMounted(() => {
      updateMenuFromQuery();
    });

    watch(() => route.query.currentMenu, (newVal) => {
      if (newVal) {
        currentMenu.value = newVal.toString();
      }
    });
    
    return {
      currentMenu
    };
  }
};
</script>

<style>
@import '@/assets/css/mypage.css';
</style> 