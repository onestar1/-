<!-- 
	2021/8/24
	YuanZhuang
	755966092@qq.com 
-->
<template>
	<view class="wrap">
		<view class="wrap" v-if="isShowList">
			<view v-if='isShowResult' class="text-white ipt-info">
				<view>
					<view class="ipt-info-item">
						昵称
						<view class="text-green">
							<input :value="nickname" placeholder="输入昵称" @input="nameChange" class="ipt-info-item-ipt" />
						</view>
					</view>
					<view class="ipt-info-item">
						生日
						<view class="text-green">
							<text @click="showCalendar">{{date}}</text>
						</view>
					</view>
					<view class="ipt-info-item">
						出生时辰
						<view class="text-green">
							<picker mode="selector" :range="timeArr" :value="timeValue" @change='timeChange'>
								{{timeArr[timeValue]}}
							</picker>
						</view>
					</view>
					<view class="ipt-info-item">
						预计寿命
						<view class="text-green">
							<picker mode="selector" :range="lifeRange" :value="lifeValue" @change='lifeChange'>
								{{lifeRange[lifeValue]}}
							</picker>
						</view>
					</view>
					<view class="ipt-info-item">
						<view>生辰八字五行<text class="text-xs text-gray">(分享须谨慎)</text></view>
						<view class="text-green">
							<switch @change="switchChange" :checked="switchBz" />
						</view>
					</view>
				</view>

				<view class="margin-top-xl response reset-btn flex justify-center">
					<view @click="showList" v-if="isShowListBtn" class="margin-right">查看列表</view>
					<view @click="viewResult">查看结果</view>
				</view>
			</view>
			<view v-else class="text-white view-info">
				<view class="response">
					<view class="result-info-item text-xxl text-bold">{{birthdayData.nickname}}</view>
					<view class="battery">
						<view class="battery-left">
							<view class="capacity-wrap">
								<view class="capacity"
									:style="{width: birthdayData.progress+'%','background-color': birthdayData.progresColor}">
								</view>
							</view>
						</view>
						<view class="battery-right"></view>
					</view>
					<view class="result-info-item text-xxl text-bold" :style="{'color': birthdayData.progresColor}">
						{{birthdayData.progress}}%
					</view>
					<view class="result-info-item text-xs">{{birthdayData.yl}}</view>
					<view class="result-info-item text-xs">{{birthdayData.nl}}</view>
					<view class="result-info-item text-xs" v-if="switchBz">{{birthdayData.bz}}</view>
					<view class="result-info-item text-xs" v-if="switchBz">{{birthdayData.wx}}</view>
					<view class="result-info-item text-xl text-bold" :style="{'color': birthdayData.progresColor}">您已经
						{{birthdayData.ages}} 岁了
					</view>
					<view class="result-info-item text-xl text-bold" :style="{'color': birthdayData.progresColor}">
						{{birthdayData.years}}岁
						{{birthdayData.cMonths}}个月 零{{birthdayData.cDays}}天
					</view>
					<view class="result-info-item">您已经在这个世界上已经存在了</view>
					<view class="margin-top-lg">
						<view class="info">
							<view class="info-item">
								<view>年</view>
								<view class="text-bold text-xl">{{birthdayData.years}}</view>
							</view>
							<view class="info-item">
								<view>月</view>
								<view class="text-bold text-xl">{{birthdayData.months}}</view>
							</view>
							<view class="info-item">
								<view>周</view>
								<view class="text-bold text-xl">{{birthdayData.weeks}}</view>
							</view>
						</view>
						<view class="info">
							<view class="info-item">
								<view>日</view>
								<view class="text-bold text-xl">{{birthdayData.days}}</view>
							</view>
							<view class="info-item">
								<view>时</view>
								<view class="text-bold text-xl">{{birthdayData.hours}}</view>
							</view>
							<view class="info-item">
								<view>分</view>
								<view class="text-bold text-xl">{{birthdayData.minutes}}</view>
							</view>
						</view>
					</view>
				</view>
				<view class="margin-top-xl response reset-btn flex justify-center">
					<view @click="showList" v-if="isShowListBtn"  class="margin-right">查看列表</view>
					<view @click="showIpt">添加数据</view>
				</view>
			</view>
		</view>

		<view class="wrap list-wrap" v-else>
			<age-item v-for="(item,index) in listData" :data="item" @itemClick="itemClickFn" :showDeleteBtn="showDeleteBtn"
				@resetList="resetListFn" />
			<view class="margin-top-xl response reset-btn padding-bottom-lg flex justify-center">
				<view @click="showIpt" class="margin-right">添加数据</view>
				<view @click="deleteData" v-if="!showDeleteBtn">删除数据</view>
				<view @click="deleteData" v-else>操作完成</view>
			</view>
		</view>

		<quick-calendar :show='isShow' :date="date" @confirmDialog="selBirthday" @closeDialog="showCalendar">
		</quick-calendar>
	</view>
</template>

<script>
	import quickCalendar from "@/components/quick-calendar/calendar.vue";
	import moment from 'moment'
	import ageItem from "./ageItem.vue"


	export default {
		data() {
			// 默认显示当前日期
			const current = new Date();
			return {
				nickname: null,
				date: current.getFullYear() +
					"/" +
					(current.getMonth() + 1) +
					"/" +
					current.getDate(),
				isShow: false,
				lifeRange: [
					"1岁",
					"2岁",
					"3岁",
					"4岁",
					"5岁",
					"6岁",
					"7岁",
					"8岁",
					"9岁",
					"10岁",
					"11岁",
					"12岁",
					"13岁",
					"14岁",
					"15岁",
					"16岁",
					"17岁",
					"18岁",
					"19岁",
					"20岁",
					"21岁",
					"22岁",
					"23岁",
					"24岁",
					"25岁",
					"26岁",
					"27岁",
					"28岁",
					"29岁",
					"30岁",
					"31岁",
					"32岁",
					"33岁",
					"34岁",
					"35岁",
					"36岁",
					"37岁",
					"38岁",
					"39岁",
					"40岁",
					"41岁",
					"42岁",
					"43岁",
					"44岁",
					"45岁",
					"46岁",
					"47岁",
					"48岁",
					"49岁",
					"50岁",
					"51岁",
					"52岁",
					"53岁",
					"54岁",
					"55岁",
					"56岁",
					"57岁",
					"58岁",
					"59岁",
					"60岁",
					"61岁",
					"62岁",
					"63岁",
					"64岁",
					"65岁",
					"66岁",
					"67岁",
					"68岁",
					"69岁",
					"70岁",
					"71岁",
					"72岁",
					"73岁",
					"74岁",
					"75岁",
					"76岁",
					"77岁",
					"78岁",
					"79岁",
					"80岁",
					"81岁",
					"82岁",
					"83岁",
					"84岁",
					"85岁",
					"86岁",
					"87岁",
					"88岁",
					"89岁",
					"90岁",
					"91岁",
					"92岁",
					"93岁",
					"94岁",
					"95岁",
					"96岁",
					"97岁",
					"98岁",
					"99岁",
					"100岁",
					"101岁",
					"102岁",
					"103岁",
					"104岁",
					"105岁",
					"106岁",
					"107岁",
					"108岁",
					"109岁",
					"110岁",
					"111岁",
					"112岁",
					"113岁",
					"114岁",
					"115岁",
					"116岁",
					"117岁",
					"118岁",
					"119岁",
					"120岁",
					"121岁",
					"122岁",
					"123岁",
					"124岁",
					"125岁",
					"126岁",
					"127岁",
					"128岁",
					"129岁",
					"130岁",
					"131岁",
					"132岁",
					"133岁",
					"134岁",
					"135岁",
					"136岁",
					"137岁",
					"138岁",
					"139岁",
					"140岁",
					"141岁",
					"142岁",
					"143岁",
					"144岁",
					"145岁",
					"146岁",
					"147岁",
					"148岁",
					"149岁",
					"150岁",
				],
				lifeValue: 84,
				timeArr: [
					"子时(23:00 - 1:00)",
					"丑时(1:00 - 3:00)",
					"寅时(3:00 - 5:00)",
					"卯时(5:00 - 7:00)",
					"辰时(7:00 - 9:00)",
					"巳时(9:00 - 11:00)",
					"午时(11:00 - 13:00)",
					"未时(13:00 - 15:00)",
					"申时(15:00 - 17:00)",
					"酉时(17:00 - 19:00)",
					"戌时(19:00 - 21:00)",
					"亥时(21:00 - 23:00)",
					"未知"
				],
				timeValue: 6,
				isShowResult: true,
				years: 0,
				months: 0,
				days: 0,
				weeks: 0,
				hours: 0,
				minutes: 0,
				ages: 0,
				cMonths: 0,
				cDays: 0,
				progress: 0,
				progresColor: "green",
				switchBz: false,
				// 是否有缓存数据，有显示列表，无缓存显示添加数据页面
				isShowList: false,
				listData: [],
				birthdayData: {},
				showDeleteBtn: false, // 删除按钮
				isShowListBtn: true
			};
		},
		components: {
			quickCalendar,
			ageItem
		},
		methods: {
			onLoad() {
				this.getStore1()
			},
			// 获取缓存
			getStore1() {
				// 查看有没有缓存
				let store = [];
				let self = this
				uni.getStorage({
					key: 'AGE_DATA',
					success: function(res) {
						store = JSON.parse(res.data)
						if (store.length > 0) {
							self.isShowList = false
							self.isShowListBtn = true
							// 有缓存
							self.getStore2(store)
						} else {
							self.isShowList = true
							self.isShowListBtn = false
						}
					},
					fail() {
						self.isShowList = true
						self.isShowListBtn = false
					}
				})
			},
			getStore2(data) {
				let listData = []
				data.forEach(item => {
					listData.push(this.jsResult({
						...item
					}))
				})
				this.listData = listData
			},

			// 刷新页面
			resetListFn() {
				this.getStore1()
			},

			// 显示删除数据按钮
			deleteData() {
				this.showDeleteBtn = !this.showDeleteBtn
			},
			
			// 点击一条数据查看详情
			itemClickFn(data) {
				this.birthdayData = data;
				// 显示详情， 隐藏列表
				this.isShowResult = false
				this.isShowList = true
			},

			// 新建一个人
			showIpt() {
				this.isShowResult = true
				this.isShowList = true
			},
			// 显示列表
			showList() {
				this.getStore1()
				this.isShowList = false
			},
			// 显示日期选择框
			showCalendar() {
				this.isShow = !this.isShow;
			},
			// 选择生日
			selBirthday(data) {
				this.showCalendar();
				this.date =
					data.date;
			},
			// 显示/隐藏生辰八字
			switchChange(data) {
				this.switchBz = data.target.value
			},
			// 选择寿命
			lifeChange(data) {
				this.lifeValue = data.detail.value;
			},
			// 选择寿命
			timeChange(data) {
				this.timeValue = data.detail.value;
			},
			// 输入昵称
			nameChange(data) {
				this.nickname = data.detail.value;
			},
			// 查看结果
			viewResult() {
				const start = moment(this.date);
				const end = moment();
				// 显示查看列表按钮
				this.isShowListBtn = true;
				// 判断输入数据是否正常
				if (!this.nickname) {
					uni.showToast({
						title: '输入昵称',
						icon: "none",
						duration: 1000
					});
				} else if (moment(end).isBefore(start)) {
					// 开始时间大于结束时间
					uni.showToast({
						title: '生日不能晚于今天',
						icon: "none",
						duration: 1000
					});
				} else {

					let obj = {
						id: moment().unix(),
						paramBirthday: this.date,
						paramNickname: this.nickname,
						paramTime: this.timeValue == 12 ? 0 : this.timeValue * 2,
						paramExpect: this.lifeRange[this.lifeValue].replace('岁', '')
					}

					let ageDataArr = []
					uni.getStorage({
						key: 'AGE_DATA',
						success: function(res) {
							ageDataArr = JSON.parse(res.data)
							ageDataArr.push(obj)
							// 设置缓存
							uni.setStorage({
								key: 'AGE_DATA',
								data: JSON.stringify(ageDataArr),
								success: function() {}
							});
						},
						fail: function() {
							ageDataArr.push(obj)
							uni.setStorage({
								key: 'AGE_DATA',
								data: JSON.stringify(ageDataArr),
								success: function() {}
							});
						}
					});


					this.birthdayData = this.jsResult({
						...obj
					})

					// 显示结果页面
					this.isShowResult = !this.isShowResult;
				}
			},
			jsResult(param) {
				const start = moment(param.paramBirthday);
				const end = moment();
				let birthday = param.paramBirthday.split("/")

				this.years = end.diff(start, 'years')
				this.months = end.diff(start, 'months')
				this.days = end.diff(start, 'days')
				this.weeks = end.diff(start, 'weeks')
				this.hours = end.diff(start, 'hours')
				this.minutes = end.diff(start, 'minutes')

				let duration = moment.duration(this.minutes, 'minutes');
				this.ages = duration.asYears().toFixed(2) // 年龄
				this.cMonths = duration.months() // 几个月
				this.cDays = duration.days() // 几天

				// 预期年龄
				let total = moment.duration(this.lifeRange[this.lifeValue].replace('岁', ''), 'years');
				// 当前年龄占比
				let progress = ((end.diff(start, 'seconds') / total.asSeconds()) * 100).toFixed(2)
				let progresColor = ''
				if (progress > 60 && progress < 90) {
					progresColor = 'orange'
				} else if (progress > 90) {
					progresColor = 'red'
				} else {
					progresColor = 'green'
				}



				// 解析生日
				let birthdatInfo = this.explainBirthday(birthday[0], birthday[1], birthday[2], this.timeValue ==
					12 ? 0 : this.timeValue * 2)

				let ageInfo = {
					id: param.id,
					years: end.diff(start, 'years'),
					months: end.diff(start, 'months'),
					days: end.diff(start, 'days'),
					weeks: end.diff(start, 'weeks'),
					hours: end.diff(start, 'hours'),
					minutes: end.diff(start, 'minutes'),
					nickname: param.paramNickname,
					...birthdatInfo,
					ages: duration.asYears().toFixed(2), // 年龄
					cMonths: duration.months(), // 年龄月份
					cDays: duration.days(), // 年龄天  18岁 8个月 18天
					progress: progress,
					progresColor: progresColor,
				}
				return ageInfo
			},
			// 解析生日
			explainBirthday(nian, yue, ri, hh) {
				// 计算所需信息
				let tg = new Array("甲", "乙", "丙", "丁", "戊", "己", "庚", "辛", "壬", "癸");
				let dz = new Array("子", "丑", "寅", "卯", "辰", "巳", "午", "未", "申", "酉", "戌", "亥");
				let sx = new Array("鼠", "牛", "虎", "兔", "龙", "蛇", "马", "羊", "猴", "鸡", "狗", "猪");
				let w = new Array("木", "火", "土", "金", "水");
				let f = new Array("东", "南", "中", "西", "北");
				let sz = new Array("一", "二", "三", "四", "五", "六", "七", "八", "九", "十",
					"十一", "十二", "十三", "十四", "十五", "十六", "十七", "十八", "十九", "二十",
					"廿一", "廿二", "廿三", "廿四", "廿五", "廿六", "廿七", "廿八", "廿九", "三十");
				let m0 = new Array(
					0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, //1901
					0, 1, 0, 1, 2, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0,
					1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 3, 0, 1, 0, 1, 0, 1, 0, 1,
					0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1,
					0, 4, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 3632)
				let m1 = new Array(
					1, 0, 1, 0, 0, 4, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, //1911
					1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 3, 0, 1, 0, 0, 1, 0, 1,
					1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0,
					1, 2, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1,
					0, 1, 0, 0, 1, 0, 3, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 7294)
				let m2 = new Array(
					1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 2, 1, 0, 0, 1, 0, 1, 1, //1921
					0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0,
					1, 0, 1, 3, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0,
					1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 4, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1,
					0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 4, 0, 1, 0, 1, 1, 0, 10955);
				let m3 = new Array(
					1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, //1931
					0, 1, 1, 0, 5, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1,
					0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 3, 0, 0, 1, 1, 0, 1, 1, 1, 0,
					1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 3, 0, 1, 1, 0, 1,
					1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 14587);
				let m4 = new Array(
					1, 1, 0, 1, 1, 3, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, //1941
					0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 3, 0, 1, 0, 1, 1, 0, 1, 1,
					0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1,
					1, 4, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1,
					1, 0, 1, 1, 0, 1, 2, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 18249);
				let m5 = new Array(
					1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 3, 0, 1, 1, 0, 1, 0, 1, //1951
					0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1,
					0, 1, 3, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1,
					1, 0, 1, 0, 1, 0, 0, 4, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1,
					0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 4, 1, 0, 1, 0, 1, 0, 21911);
				let m6 = new Array(
					1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, //1961
					1, 0, 1, 2, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1,
					0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 4, 1, 0, 0, 1, 0, 0, 1, 1, 0,
					1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 4, 1, 0, 1, 0, 1,
					0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 25544);
				let m7 = new Array(
					0, 1, 0, 0, 4, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, //1971
					1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 4, 0, 1, 0, 0, 1, 1, 0, 1,
					1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 4, 0, 1, 0, 1,
					1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0,
					1, 0, 0, 1, 0, 5, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 29206);
				let m8 = new Array(
					0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 2, 1, 0, 0, 1, 0, 1, 1, 1, //1981
					1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 4, 1, 1,
					0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0,
					1, 0, 1, 0, 1, 4, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0,
					1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 4, 0, 1, 0, 1, 1, 1, 1, 32868);
				let m9 = new Array(
					0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, //1991
					0, 1, 4, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1,
					0, 1, 1, 0, 1, 0, 1, 4, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0,
					1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 1, 2, 1, 1, 0, 1, 1, 0, 1,
					1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 36499);
				let m10 = new Array(
					1, 1, 0, 4, 1, 0, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, //2001
					1, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 4, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1,
					0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 4, 1, 1, 0, 1, 1,
					0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1,
					1, 1, 0, 0, 4, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 40161);
				let m11 = new Array(
					1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 4, 1, 0, 1, 0, 1, 0, 1, 0, //2011
					1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 4, 1, 0, 1,
					0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1,
					0, 1, 0, 1, 0, 3, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1,
					1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 4, 1, 0, 0, 1, 0, 1, 0, 1, 43823);
				let m12 = new Array(
					0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, //2021
					0, 4, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0,
					1, 0, 1, 0, 0, 4, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0,
					1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 4, 0, 1, 0, 0, 1, 1, 0,
					1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 47455)
				let m13 = new Array(
					0, 1, 4, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, //2031
					0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 4, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1,
					1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 3, 0, 0, 1, 0, 1, 1,
					1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0,
					1, 1, 0, 1, 4, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 51117)
				let m14 = new Array(
					0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 4, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, //2041
					0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 2, 1, 0, 1, 1, 1,
					1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1,
					1, 0, 1, 1, 3, 0, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0,
					1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 1, 3, 0, 1, 0, 1, 1, 0, 1, 1, 0, 54779)
				let ms = new Array(m0, m1, m2, m3, m4, m5, m6, m7, m8, m9, m10, m11, m12, m13, m14);
				let ly = new Array(29, 30, 58, 59, 59, 60);
				let tw = new Array(0, 0, 1, 1, 2, 2, 3, 3, 4, 4);
				let dw = new Array(4, 2, 0, 0, 2, 1, 1, 2, 3, 3, 2, 4);

				// 计算
				let y = nian; // 年
				let gl0 = (Date.UTC(y, 0, 1) - Date.UTC(1901, 1, 19)) / 86400000;
				let dy = y - 1901;
				let i = Math.floor(dy / 10);

				let m = yue; // 月
				let d = ri;
				let h = hh;
				let j = 0;

				let sum = (Date.UTC(y, m - 1, d, h) - Date.UTC(1901, 1, 18, 23)) / 1000;
				let sumd = Math.floor(sum / 86400);
				let day = (Math.floor((sum - 1800) / 86400) + 51) % 7;
				let xq = (day == 0) ? "日" : sz[day - 1]; // 星期
				let tgr = (sumd + 54) % 10;
				let dzr = (sumd + 52) % 12;
				let gzr = tg[tgr] + dz[dzr];
				let dzs = Math.floor((h * 1 + 1) / 2) % 12;
				let tgs = ((tgr % 5) * 2 + dzs) % 10;
				let gzs = tg[tgs] + dz[dzs];
				let k = (i == 0) ? 0 : ms[i - 1][120];
				let p = i * 120;
				for (j = 0; j < 120; j++) {
					k += ly[ms[i][j]];
					p++;
					if (k > sumd) break;
				}
				if (sumd + 30 < 0) {
					ri = 59 + sumd;
					p = -1;
				} else if (sumd < 0) {
					ri = 30 + sumd;
					p = 0;
				} else {
					ri = sumd + ly[ms[i][j]] - k;
				}
				yue = ((p + 11) % 12 == 0) ? "正" : sz[(p + 11) % 12];
				let mij = ms[i][j];
				if ((mij == 2 || mij == 3) && ri > 28) {
					ri -= 29;
					yue = "闰" + yue;
				} else if ((mij == 4 || mij == 5) && ri > 29) {
					ri -= 30;
					yue = "闰" + yue;

				}
				ri = ((ri < 10) ? "初" : "") + sz[ri];
				let tgn = Math.floor((p - 1) / 12 + 7) % 10;
				let dzn = Math.floor((p - 1) / 12 + 1) % 12;
				let gzn = tg[tgn] + dz[dzn];
				let tgy = (p + 5) % 10;
				let dzy = (p + 1) % 12;
				let gzy = tg[tgy] + dz[dzy];
				let tn = tw[tgn];
				let dn = dw[dzn];
				let ty = tw[tgy];
				dy = dw[dzy];
				let tr = tw[tgr];
				let dr = dw[dzr];
				let ts = tw[tgs];
				let ds = dw[dzs];


				return {
					bz: gzn + ' ' + gzy + ' ' + gzr + ' ' + gzs,
					wx: w[tn] + w[dn] + " " + w[ty] + w[dy] + " " + w[tr] + w[dr] + " " + w[ts] + w[ds],
					fw: f[tn] + f[dn] + " " + f[ty] + f[dy] + " " + f[tr] + f[dr] + " " + f[ts] + f[ds],
					sx: sx[dzn],
					yl: y + "-" + m + "-" + d + " 星期" + xq,
					nl: gzn + "年 " + yue + "月 " + ri
				}
			}
		},
	};
</script>

<style>
	page {
		background-color: #2C405A;
		box-sizing: border-box;
		padding: 20px 30px;
		height: 100%;

	}

	.wrap {
		height: 100%;
	}

	.ipt-info {
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}

	.view-info {
		height: 100%;
		flex-direction: column;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.info {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		margin: 10px 0;
	}

	.info-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 30%
	}

	.result-info-item {
		text-align: center;
		margin: 10px 0;
	}

	.ipt-info-item {
		margin: 20px 0;
		display: flex;
		justify-content: space-between;
	}

	.ipt-info-item-ipt {
		text-align: right;
	}

	.battery {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}

	.battery-left {
		width: 150px;
		height: 80px;
		border: 5px solid #fff;
		background-color: #fff;
		border-radius: 10px;
	}

	.battery-right {
		width: 12px;
		height: 30px;
		background-color: #fff;
		border-top-right-radius: 5px;
		border-bottom-right-radius: 5px;
	}

	.capacity-wrap {
		width: 100%;
		height: 100%;
		background-color: #2C405A;
		border-radius: 10px;
	}

	.capacity {
		background-color: #1CBBB4;
		width: 90%;
		height: 100%;
		border: 3px solid #2C405A;
		border-radius: 10px;
	}

	.reset-btn {
		text-align: center;
		color: gray;
		text-decoration: underline;
	}

	/* 列表 */
	.list-wrap {
		padding-bottom: 20px;
	}
</style>
