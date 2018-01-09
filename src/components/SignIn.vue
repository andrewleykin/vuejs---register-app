<template>
	<form class="mt-5" @submit.prevent="enterUser">
		<div class="form-group">
			<label for="email">Ваш email:</label>
			<input v-model="user.email" type="email" placeholder="Введите email:*" class="form-control" id="email" required>
		</div>
		<div class="form-group">
			<label for="password">Ваш пароль (минимум 6 символов):</label>
			<input v-model="user.password" type="password" placeholder="Введите пароль:*" class="form-control" id="password" required>
		</div>
		<button class="btn btn-primary" type="submit">Войти</button>
	</form>
</template>

<script>
	export default {
		name: 'sign-in',
		data() {
			return {
				user: {
					email: '',
					password: ''
				}
			}
		},
		methods: {
			enterUser() {
				firebase.auth().signInWithEmailAndPassword(this.user.email, this.user.password)
					.then( response => {
						const sett = {
							email: response.email,
							uid: response.uid,
							mainPage: true,
							complete: true
						}
						this.$emit('addUser', sett);
					});
			}
		}
	}
</script>