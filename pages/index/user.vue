<!-- 个人中心 -->
<template>
	<view class="personal-wrap">
		<!-- 个人信息卡片 -->
		<userinfo-card v-if="userHeadData && userHeadData.style" :scrollTop="scrollTop" :detail="userHeadData" @onShare="onShare"></userinfo-card>
		<!-- 登录提示 -->
		<shopro-auth-modal></shopro-auth-modal>
	</view>
</template>

<script>
import shBanner from './components/sh-banner.vue';
import shGridSwiper from './components/sh-grid-swiper.vue';
import shAdv from './components/sh-adv.vue';
import shRichtext from './components/sh-richtext.vue';
import shCell from './components/sh-cell.vue';
import shGrid from './components/sh-grid.vue';
import shOrderCard from './components/sh-order-card.vue';
import shWallet from './components/sh-wallet.vue';
import shHotGoods from './components/sh-hot-goods.vue';

import userinfoCard from './user/userinfo-card.vue';

import { mapMutations, mapActions, mapState, mapGetters } from 'vuex';

import share from '@/shopro/share';

export default {
	components: {
		shBanner,
		shGridSwiper,
		shAdv,
		shRichtext,
		shCell,
		shGrid,
		shWallet,
		shOrderCard,
		shHotGoods,

		userinfoCard
	},
	data() {
		return {
			scrollTop: 0,
			showShare: false,
			enable: false //是否开启吸顶。
		};
	},
	computed: {
		...mapGetters(['initShop', 'userTemplate', 'isLogin', 'userInfo', 'authType']),
		userHeadData() {
			if (this.userTemplate?.length) {
				return this.userTemplate[0].content;
			}
		}
	},
	// 下拉刷新
	onPullDownRefresh() {
		this.init();
	},
	onPageScroll(e) {
		this.scrollTop = e.scrollTop;
	},

	onShow() {
		if (this.isLogin) {
			this.init();
			this.getUserData();
		}
		this.enable = true;
	},

	methods: {
		...mapActions(['getUserInfo', 'showAuthModal', 'getUserData']),
		onShare() {
			this.showShare = true;
			uni.hideTabBar();
		},
		// 初始化
		init() {
			this.getUserInfo()
				.then(res => {
					uni.stopPullDownRefresh();
				})
				.catch(e => {
					uni.stopPullDownRefresh();
				});
		}
	}
};
</script>

<style lang="scss">
.copyright-box {
	.copyright-text {
		font-size: 22rpx;
		font-weight: 500;
		color: #c4c4c4;
	}
}
</style>
