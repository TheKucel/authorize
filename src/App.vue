<template>
	<div id="app">
		<section :class="{ correct: login.correct }">
			<div class="back-container">
				<h1>
					<span v-for="(l, k) in 'WELCOME'" :key="k">{{ l }}</span>
				</h1>
				<img src="/static/imgs/back.png" alt="" />
			</div>
			<transition-group name="fade">
				<template v-if="!login.correct">
					<h2 :key="1">Sign up</h2>
					<form :key="2" class="form-container">
						<input
							v-model="user.login"
							type="text"
							placeholder="Login"
							:class="{
								uncorrect: login.errorComponent === 'login',
							}"
							@focus="login.errorComponent = ''"
						/>
						<input
							v-model="user.password"
							type="password"
							placeholder="Password"
							:class="{
								uncorrect: login.errorComponent === 'password',
							}"
							@focus="login.errorComponent = ''"
						/>
					</form>
					<button :key="3" @click="loginEmulate">
						{{
							login.errorComponent ? "Данные не верны" : "Sign up"
						}}
					</button>
				</template>
			</transition-group>
		</section>
	</div>
</template>

<script>
export default {
	name: "App",
	data() {
		return {
			user: {
				login: "",
				password: "",
			},
			correctData: {
				login: "admin",
				password: "userPassword",
			},
			login: {
				correct: false,
				errorComponent: "",
			},
		};
	},
	methods: {
		loginEmulate() {
			const u = this.user;
			const c = this.correctData;
			if (u.login !== c.login) {
				this.login.errorComponent = "login";
			} else if (u.password !== c.password) {
				this.login.errorComponent = "password";
			}
			this.login.correct = !this.login.errorComponent;
		},
	},
};
</script>

<style lang="scss">
@import "./config.scss";

// keyframes

@keyframes jumpUp {
	30%,
	50% {
		width: ptr(400px);
		height: ptr(540px);
		border-radius: ptr(20px);
	}
	100% {
		width: 100%;
		height: 100%;
		border-radius: 0;
	}
}

@keyframes fade {
	100% {
		opacity: 0;
	}
}

@keyframes fade-in {
	0% {
		opacity: 0;
		transform: translateY(ptr(60px));
	}
	100% {
		opacity: 1;
	}
}

// main styles

#app {
	font-family: sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	display: flex;
	justify-content: center;
	align-items: center;
	height: 100vh;
}

section {
	justify-self: center;
	align-self: center;
	border-radius: ptr(23px);
	width: ptr(447px);
	height: ptr(600px);
	display: grid;
	justify-items: center;
	grid-auto-rows: max-content;
	padding-top: ptr(102px);
	box-sizing: border-box;
	position: relative;
	overflow: hidden;
	background: #bbc5c6;
}

.back-container {
	width: 100%;
	height: 100%;
	position: absolute;
	display: flex;
	align-items: center;
	justify-content: center;
	h1 {
		font-size: ptr(200px);
		font-family: "Druk Wide Medium Cy", sans-serif;
		font-weight: 500;
		color: white;
		position: absolute;
		span {
			display: inline-block;
			opacity: 0;
		}
	}
	img {
		width: 100vw;
		height: 100%;
		object-fit: cover;
		position: relative;
	}
}

h2 {
	font-size: ptr(34px);
	margin: 0;
	font-weight: 700;
	position: relative;
	text-align: center;
	color: white;
}

button {
	display: flex;
	align-items: center;
	justify-content: center;
	border: none;
	padding: 0;
	border-radius: 100px;
	width: ptr(298px);
	height: ptr(63px);
	background: #00000080;
	font-weight: 500;
	color: white;
	backdrop-filter: blur(5px);
	cursor: pointer;
	transition: 0.1s cubic-bezier(0.65, 0, 0.35, 1);
	&:hover {
		backdrop-filter: blur(12px);
	}
}

.form-container {
	display: grid;
	overflow: hidden;
	margin: ptr(63px) 0 ptr(44px);
	border-radius: ptr(16px);
	background: rgba(255, 255, 255, 0.5);
	backdrop-filter: blur(30px);
	width: ptr(298px);
	input {
		border: none;
		margin: 0;
		height: ptr(56px);
		padding: 0 ptr(18px);
		font-weight: 600;
		background: none;
		font-size: ptr(16px);
		font-family: sans-serif;
		&::placeholder {
			color: #5b5b5b;
		}
		&:focus {
			outline: none;
		}
	}
}

.uncorrect {
	color: rgb(173, 53, 53);
}

.correct {
	animation: jumpUp 3s forwards;
	.back-container {
		h1 {
			@for $i from 1 through 7 {
				span:nth-child(#{$i}) {
					animation: fade-in 0.9s #{$i * 0.07s + 2.5s} forwards;
				}
			}
		}
	}
}

.fade-leave-active {
	animation: fade 0.4s;
}
</style>
