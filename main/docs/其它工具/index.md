---
layout: page
---
<script setup>
import {
  VPTeamPage,
  VPTeamPageTitle,
  VPTeamMembers,
  VPTeamPageSection
} from 'vitepress/theme'
import mo from './图标/默认.jpg'
import baige from './图标/百鸽.jpg'

const coreMembers = [
	{
		avatar: baige, name: "百鸽", title: "游戏王查卡工具",
		org: "点击跳转", orgLink: "https://ygocdb.com/", decs: "", sponsor: "", actionText: "",
		links: [],
	},{
		avatar: mo, name: "Master Duel 卡组识别", title: "游戏王卡组识别",
		org: "点击跳转", orgLink: "https://get-deck.com/", decs: "", sponsor: "", actionText: "",
		links: [{icon: "github", link: "https://github.com/Souls-R/getdeck"}],
	},
];

</script>

<VPTeamPage>
  <VPTeamPageTitle>
    <template #title>其它游戏王工具</template>
    <template #lead>引用链接</template>
  </VPTeamPageTitle>
  <VPTeamMembers size="medium" :members="coreMembers" />
</VPTeamPage>