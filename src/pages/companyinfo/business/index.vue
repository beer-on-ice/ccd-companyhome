<template lang="pug">
  .business-info-wrapper
    .business-info-top-wrapper
      h2 华设资产管理（上海）有限公司
      ul.flex-b
        li(v-for="(item,index) in topBtnGroup" :key="item.id" @click="currentSelect = index" :class="currentSelect===index?'active':''")
          p {{item.num}}
          p {{item.name}}
    p.noInfo(v-if="noInfo") 暂无相关信息
    keep-alive(v-if="!noInfo")
      component(v-bind:is="currentTabComponent[currentSelect]")
</template>
<script>
import BusinessCom from './component/BusinessCom'
import ShareholderCom from './component/ShareholderCom'
import MainMemberCom from './component/MainMemberCom'
import BranchCom from './component/BranchCom'
export default {
  components: {
    BusinessCom,
    ShareholderCom,
    MainMemberCom,
    BranchCom
  },
  data () {
    return {
      currentSelect: 0,
      noInfo: false,
      currentTabComponent: [
        'business-com',
        'shareholder-com',
        'main-member-com',
        'branch-com'
      ],
      topBtnGroup: [
        {
          id: 0,
          num: 'ALL',
          name: '工商信息'
        },
        {
          id: 1,
          num: '8',
          name: '股东信息'
        },
        {
          id: 2,
          num: '6',
          name: '主要人员'
        },
        {
          id: 3,
          num: '4',
          name: '分支机构'
        }
      ]
    }
  }
}
</script>

<style lang="scss">
.business-info-wrapper {
	padding-top: 40px;
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	overflow-y: scroll;
	.business-info-top-wrapper {
		padding: 0 36px 20px;
		h2 {
			font-size: 42px;
			line-height: 48px;
			color: #333333;
			font-weight: bold;
		}
		ul {
			margin-top: 50px;
			li {
				text-align: center;
				padding: 0 24px;
				p {
					font-size: 30px;
					line-height: 48px;
					font-weight: 600;
					color: #333333;
					&:nth-of-type(2) {
						font-size: 24px;
						font-weight: 500;
					}
				}
				&.active {
					background: #f5f5f5;
				}
			}
		}
	}
	.noInfo {
		font-size: 32px;
		line-height: 50px;
		color: #666;
		font-weight: 500;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
}
</style>
