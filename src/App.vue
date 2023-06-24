<template>
	<div @click="closeHeader()" class="app__wrapper">
		<Header
			@toggleBurgerMenu="toggleBurgerMenu"
			:isBurgerOpen="isBurgerOpen"
		></Header>
		<main class="main">
			<section class="promo">
				<div class="promo__container">
					<div class="promo__content">
						<div class="promo__image">
							<img
								src="@/assets/images/illustration-working.svg"
								alt="illustration"
							/>
						</div>
						<div class="promo__body">
							<h1 class="promo__title">More than just shorter links</h1>
							<p class="promo__text">
								Build your brand’s recognition and get detailed insights on how
								your links are performing.
							</p>
							<a
								href="#action"
								class="promo__button"
								v-smooth-scroll="{ offset: -10 }"
							>
								Get Started
							</a>
						</div>
					</div>
				</div>
			</section>
			<section class="shortening">
				<div class="shortening__container">
					<div class="shortening__content">
						<div class="shortening__action action" id="action">
							<div class="action__data">
								<input
									type="text"
									class="action__input"
									:class="{ error: isUrlEmpty }"
									placeholder="Shorten a link here..."
									v-model="url"
									v-on:keyup.enter="shorteningUrl"
								/>
								<p class="action__input-note">Please add a link</p>
							</div>
							<button class="action__button" @click="shorteningUrl">
								Shorten It!
							</button>
						</div>
						<div class="shortening__results">
							<transition-group :name="transitionName">
								<Result
									v-for="result in results"
									:key="result.id"
									:result="result"
								/>
							</transition-group>
						</div>
					</div>
				</div>
			</section>
			<section class="statistic">
				<div class="statistic__container">
					<div class="statistic__content">
						<h2 class="statistic__title">Advanced Statistics</h2>
						<p class="statistic__text">
							Track how your links are performing across the web with our
							advanced statistics dashboard.
						</p>
						<div class="statistic__cards">
							<div class="statistic__card card">
								<div class="card__icon">
									<img
										src="@/assets/images/icon-brand-recognition.svg"
										alt="icon"
									/>
								</div>
								<p class="card__title">Brand Recognition</p>
								<p class="card__text">
									Boost your brand recognition with each click. Generic links
									don’t mean a thing. Branded links help instil confidence in
									your content.
								</p>
							</div>
							<div class="statistic__card card">
								<div class="card__icon">
									<img
										src="@/assets/images/icon-detailed-records.svg"
										alt="icon"
									/>
								</div>
								<p class="card__title">Detailed Records</p>
								<p class="card__text">
									Gain insights into who is clicking your links. Knowing when
									and where people engage with your content helps inform better
									decisions.
								</p>
							</div>
							<div class="statistic__card card">
								<div class="card__icon">
									<img
										src="@/assets/images/icon-fully-customizable.svg"
										alt="icon"
									/>
								</div>
								<p class="card__title">Fully Customizable</p>
								<p class="card__text">
									Improve brand awareness and content discoverability through
									customizable links, supercharging audience engagement.
								</p>
							</div>
						</div>
					</div>
				</div>
			</section>
			<section class="boost">
				<div class="boost__container">
					<p class="boost__title">Boost your links today</p>
					<a
						href="#action"
						class="boost__button"
						v-smooth-scroll="{ offset: -10 }"
						>Get Started</a
					>
				</div>
			</section>
		</main>
		<Footer />
	</div>
</template>

<script setup>
import Footer from "@/components/Footer.vue"
import Header from "@/components/Header.vue"
import Result from "@/components/ShortenedItem.vue"
import { onMounted, ref } from "vue"
const isBurgerOpen = ref(false)

// Menu
function toggleBurgerMenu() {
	isBurgerOpen.value = !isBurgerOpen.value
}
function closeHeader() {
	isBurgerOpen.value = false
}
window.addEventListener("resize", function () {
	if (window.innerWidth >= 768) {
		closeHeader()
	}
})

// Shortening
const results = ref([])
const url = ref("")
const isUrlEmpty = ref(false)
const id = ref(0)
const transitionName = ref("")

async function shorteningUrl() {
	if (!url.value) {
		isUrlEmpty.value = true
		return false
	}
	isUrlEmpty.value = false
	id.value += 1
	transitionName.value = "fade"
	let request = await fetch(
		`https://api.shrtco.de/v2/shorten?url=${encodeURIComponent(url.value)}`
	)
	let response = await request.json()
	let shortened = response.result.full_short_link
	results.value.unshift({
		id: id.value,
		linkOrig: url.value,
		linkShort: shortened
	})
	url.value = ""
	localStorage.setItem("results", JSON.stringify(results.value))

	if (results.value.length >= 10) {
		results.value.pop()
	}
}

onMounted(() => {
	if (window.localStorage.getItem("results")) {
		let res = JSON.parse(window.localStorage.getItem("results"))
		let lastid = res[0].id
		id.value = lastid
		results.value = res
	}
})
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@500;700&display=swap");
@import "@/assets/scss/index.scss";

.app {
	height: 100%;
	&__wrapper {
		min-height: 100%;
		display: flex;
		flex-direction: column;
		overflow: hidden;
	}
}
.main {
	flex: 1 1 auto;
}

// ## Sections
// promo
.promo {
	&__container {
		@extend %cnt;
		padding-top: 15px;
		padding-bottom: 170px;
	}

	&__content {
	}

	&__image {
		text-align: center;
		margin-bottom: 40px;
		& img {
			@extend %img;
			width: 150%;
			max-width: 495px;
		}
	}

	&__body {
		text-align: center;
	}

	&__title {
		font-size: 40px;
		font-weight: 700;
		color: $VeryDarkViolet;
		margin-bottom: 15px;
	}

	&__text {
		font-weight: 500;
		line-height: 165%;
		margin-bottom: 30px;
	}

	&__button {
		@include btn(198px, 56px, 20px);
		border-radius: 30px;
		font-weight: 700;
		margin-inline: auto;
	}

	@media ($tablet) {
		&__container {
			padding-top: 55px;
			padding-bottom: 112px;
		}
		&__content {
			display: flex;
			flex-direction: row-reverse;
			align-items: center;
			gap: 22px;
		}
		&__image {
			& img {
				max-width: max-content;
			}
		}
		&__body {
			text-align: left;
		}
		&__button {
			margin-inline: initial;
		}
	}
	@media ($desktop) {
		&__body {
			margin-top: -44px;
		}
		&__title {
			font-size: 80px;
			line-height: 112%;
			letter-spacing: -2px;
			margin-bottom: 0;
		}
		&__text {
			max-width: 550px;
			font-size: 22px;
			margin-bottom: 34px;
		}
	}
	@media (min-width: 1200px) {
		&__image {
			& img {
				width: 180%;
			}
		}
	}
}

// shortening
.shortening {
	background: $LightGray;
	padding-bottom: 10px;
	&__container {
		@extend %cnt;
		transform: translateY(-80px);
	}

	&__content {
	}

	&__action {
	}

	&__results {
	}

	&__result {
		margin-top: 22px;
	}

	@media ($tablet) {
		padding-bottom: 20px;
		&__container {
			transform: translateY(-84px);
		}
		&__result {
			&:first-child {
				margin-top: 26px;
			}
			margin-top: 15px;
		}
	}
}

.action {
	display: flex;
	flex-direction: column;
	align-items: center;
	gap: 15px;
	background: url("@/assets/images/bg-shorten-mobile.svg") top right no-repeat,
		$DarkViolet;
	border-radius: 10px;
	padding: 24px;

	&__data {
		width: 100%;
	}

	&__input {
		width: 100%;
		height: 48px;
		font-size: 16px;
		font-weight: 500;
		color: $VeryDarkBlue;
		border-radius: 5px;
		outline: none;
		padding: 0 15px;
		&::placeholder {
			font-size: 16px;
			font-weight: 500;
			color: $GrayishViolet;
		}
		&-note {
			display: none;
		}

		&.error {
			border: 2px solid $Red;
			&::placeholder {
				font-weight: normal;
				color: $Red;
				opacity: 0.6;
			}

			& + .action__input-note {
				display: block;
				font-size: 12px;
				font-style: italic;
				color: $Red;
				margin-top: 8px;
			}
		}
	}

	&__button {
		@include btn(280px, 48px, 18px);
		border-radius: 5px;
	}

	@media ($tablet) {
		align-items: flex-start;
		flex-direction: row;
		gap: 24px;
		background: url("@/assets/images/bg-shorten-desktop.svg") top right
				no-repeat,
			$DarkViolet;
		padding: 50px 64px;

		&__input {
			position: relative;
			height: 65px;
			font-size: 23px;
			border-radius: 10px;
			padding: 0 33px;
			&::placeholder {
				font-size: 20px;
			}
			&.error {
				& + .action__input-note {
					position: absolute;
					font-size: 16px;
				}
			}
		}

		&__button {
			max-width: 188px;
			height: 65px;
			font-weight: 700;
			font-size: 20px;
			border-radius: 10px;
		}
	}
}

// statistic
.statistic {
	background: $LightGray;
	padding-bottom: 80px;

	&__container {
		@extend %cnt;
	}

	&__content {
		text-align: center;
	}

	&__title {
		font-size: 27px;
		font-weight: 700;
		color: $VeryDarkViolet;
		margin-bottom: 24px;
	}

	&__text {
		font-size: 16px;
		font-weight: 500;
		line-height: 170%;
	}

	&__cards {
		display: flex;
		flex-direction: column;
		gap: 90px;
		margin-top: 90px;
	}

	&__card {
	}

	@media ($tablet) {
		padding-top: 23px;
		padding-bottom: 120px;
		&__title {
			font-size: 38px;
			margin-bottom: 20px;
		}
		&__text {
			max-width: 540px;
			font-size: 18px;
			line-height: 165%;
			margin-inline: auto;
		}
	}
	@media ($desktop) {
		&__cards {
			height: 356px;
			flex-direction: row;
			gap: 30px;
			margin-top: 100px;
		}
		&__card {
			max-width: 350px;
			height: max-content;
			flex: 1 1 33.333%;
		}
	}
}

.card {
	position: relative;
	background: $White;
	border-radius: 5px;
	padding: 80px 30px 40px 30px;
	&:nth-child(2) {
		position: relative;
		padding: 80px 30px 33px 30px;
		&::before,
		&::after {
			content: "";
			position: absolute;
			width: 8px;
			height: 90px;
			left: 50%;
			transform: translateX(-50%);
			background: $Cyan;
		}
		&::before {
			top: -90px;
		}
		&::after {
			bottom: -90px;
		}
	}

	&__icon {
		width: 88px;
		height: 88px;
		position: absolute;
		top: 0;
		left: 50%;
		transform: translate(-50%, -50%);
		display: flex;
		align-items: center;
		justify-content: center;
		background: $DarkViolet;
		border-radius: 50%;
	}

	&__title {
		font-size: 22px;
		font-weight: 700;
		color: $VeryDarkViolet;
		margin-bottom: 20px;
	}

	&__text {
		font-size: 15px;
		font-weight: 500;
		line-height: 170%;
	}

	@media ($desktop) {
		text-align: left;
		&:first-child {
			align-self: flex-start;
		}
		&:nth-child(2) {
			padding: 80px 30px 40px 30px;
			align-self: center;
			&::before,
			&::after {
				width: 30px;
				height: 8px;
				top: 40%;
				bottom: auto;
				transform: translateX(0);
			}
			&::before {
				left: -30px;
			}
			&::after {
				left: auto;
				right: -30px;
			}
		}
		&:last-child {
			align-self: flex-end;
		}

		&__icon {
			transform: translateY(-50%);
			left: 30px;
		}
	}
}

// boost
.boost {
	background: url("@/assets/images/bg-boost-mobile.svg") 100% / cover no-repeat,
		$DarkViolet;

	&__container {
		text-align: center;
		padding: 100px 10px 90px;
	}

	&__title {
		font-size: 27px;
		font-weight: 700;
		line-height: 1.3;
		color: $White;
		margin-bottom: 20px;
	}

	&__button {
		@include btn(198px, 56px, 20px);
		border-radius: 30px;
		margin-inline: auto;
	}

	@media ($tablet) {
		background: url("@/assets/images/bg-boost-desktop.svg") center / cover
				no-repeat,
			$DarkViolet;
		&__container {
			padding: 60px 10px 58px;
		}
		&__title {
			font-size: 38px;
			margin-bottom: 26px;
		}
	}
}

// animates
.fade-enter-active,
.fade-leave-active {
	opacity: 1;
	transition: all 0.8s ease;
	transform: translateY(0);
}

.fade-enter-from,
.fade-leave-to {
	opacity: 0;
	transition: all 0.8s ease;
	transform: translateY(-100%);
}
</style>
