<template>
	<header class="header">
		<div class="header__container">
			<div class="header__content">
				<div class="header__logo">
					<img src="@/assets/images/logo.svg" alt="logo" />
				</div>
				<div class="header__burger" @click.stop="$emit('toggleBurgerMenu')">
					<span></span>
					<span></span>
					<span></span>
				</div>
				<nav
					@click.stop
					class="header__body"
					:class="props.isBurgerOpen ? 'show' : ''"
				>
					<ul class="header__menu">
						<li class="header__menu-item">
							<a href="#" class="header__menu-link">Features</a>
						</li>
						<li class="header__menu-item">
							<a href="#" class="header__menu-link">Pricing</a>
						</li>
						<li class="header__menu-item">
							<a href="#" class="header__menu-link">Resources</a>
						</li>
					</ul>
					<ul class="header__actions">
						<li class="header__action">
							<a href="#" class="header-login">Login</a>
						</li>
						<li @click.stop="doLog" class="header__action">
							<a href="#" class="header-signup">Sign Up</a>
						</li>
					</ul>
				</nav>
			</div>
		</div>
	</header>
</template>

<script setup>
const props = defineProps({
	isBurgerOpen: Boolean
})
</script>

<style lang="scss">
@import "@/assets/scss/index.scss";

.header {
	position: relative;
	&__container {
		@extend %cnt;
		padding-top: 40px;
		padding-bottom: 24px;
	}

	&__content {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	&__logo {
	}

	&__burger {
		cursor: pointer;
		& span {
			display: block;
			width: 24px;
			height: 3px;
			background-color: $GrayishViolet;
			&:not(:last-child) {
				margin-bottom: 5px;
			}
		}
	}

	&__body {
		position: absolute;
		z-index: 99;
		width: calc(100% - 50px);
		top: -100vh;
		text-align: center;
		background: $DarkViolet;
		border-radius: 8px;
		transition: all 0.3s ease 0s;
		padding: 30px 25px 40px;
		&.show {
			top: 100%;
		}
		& * {
			color: $White;
		}
		& a {
			transition: all 0.3s ease 0s;
      @media ($mouse-device) {
        &:hover {
          color: $Cyan;
        }
      }
		}
	}

	&__menu {
		display: flex;
		flex-direction: column;
		gap: 16px;
		border-bottom: 1px solid $GrayishViolet;
		padding-bottom: 23px;
	}

	&__menu-item {
	}

	&__menu-link {
		display: inline-block;
		width: 100%;
		font-weight: 500;
		padding-block: 10px;
	}

	&__actions {
		padding-top: 24px;
	}

	&__action {
		& .header-login {
			display: inline-block;
			width: 100%;
			font-weight: 500;
			padding-block: 10px;
			margin-bottom: 18px;
		}
		& .header-signup {
			@include btn(280px, 48px, 18px);
			border-radius: 30px;
			margin-inline: auto;
		}
	}

	@media ($tablet) {
		&__content {
			gap: 40px;
		}

		&__burger {
			display: none;
		}

		&__body {
			position: static;
			flex: 1 1 auto;
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size: 15px;
			font-weight: 700;
			color: $GrayishViolet;
			background-color: transparent;
			padding: 0;
			& a {
				color: $GrayishViolet;
        @media ($mouse-device) {
          &:hover {
            color: $VeryDarkBlue;
          }
        }
			}
		}

		&__menu {
			flex-direction: row;
			gap: 33px;
			border-bottom: none;
			padding-bottom: 0;
		}

		&__menu-item {
		}

		&__menu-link {
		}

		&__actions {
			display: flex;
			gap: 40px;
			align-items: center;
			padding-top: 0;
		}

		&__action {
			& .header-login {
				margin-bottom: 0;
			}

			& .header-signup {
				width: 104px;
				max-height: 40px;
				font-size: 15px;
			}
		}
	}
	@media ($desktop) {
		&__container {
			padding-top: 48px;
		}
		&__content {
			gap: 48px;
		}
	}
}
</style>
