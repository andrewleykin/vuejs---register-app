<template>
	<form class="mt-5" @submit.prevent="registerUser">
		<div class="form-group">
			<label for="email">Ваш email:</label>
			<input v-model="user.email" type="email" placeholder="Введите email:*" class="form-control" id="email" required>
		</div>
		<div class="form-group">
			<label for="password">Ваш пароль (минимум 6 символов):</label>
			<input v-model="user.password" type="password" placeholder="Введите пароль:*" class="form-control" id="password" required>
		</div>
		<div class="form-group">
			<label for="password2">Повторите пароль:</label>
			<input v-model="user.confirmPassword" type="password" placeholder="Повторите пароль:*" class="form-control" id="password2" required>
		</div>
		<div class="alert alert-danger" role="alert" v-if="error">
			<strong>Упс!</strong> Пароли не совпадают или вы забыли их ввести
		</div>
		<button class="btn btn-primary" type="submit">Зарегистрироваться</button>
	</form>
</template>

<script>
	export default {
		name: 'sign-up',
		data() {
			return {
				user: {
					email: '',
					password: '',
					confirmPassword: ''
				},
				error: false
			}
		},
		methods: {
			registerUser() {

				if(this.user.password !== this.user.confirmPassword || this.user.password.length < 6) {
					this.error = true;
				} else {
					firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.password)
					.then( () => {
						this.$emit('reqSuccess', 'sign-in');
					})
					.catch( error => {
						console.log(error);
					})
				}
			}
		}
	}
</script>