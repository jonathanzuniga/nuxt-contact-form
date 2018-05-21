<template>
	<form class="form">
	<!-- <form v-on:submit.prevent="submitForm"> -->
		<p v-bind:class="{ 'fld-error': $v.form.name.$error }">
			<label>Name</label><br>
			<input type="text" v-model.trim="form.name" @input="$v.form.name.$touch()">

			<span class="msg-error" v-if="!$v.form.name.required">
				<small>Field is required</small>
			</span>
			<span class="msg-error" v-if="!$v.form.name.minLength">
				<small>Name must have at least {{ $v.form.name.$params.minLength.min }} letters.</small>
			</span>
		</p>

		<p v-bind:class="{ 'fld-error': $v.form.job.$error }">
			<label>Job</label><br>
			<input type="text" v-model="form.job" @input="$v.form.job.$touch()">

			<span class="msg-error" v-if="!$v.form.job.required">
				<small>Field is required</small>
			</span>
		</p>

		<p>
			<button type="submit" @click.prevent="submitForm" :disabled="$v.form.$invalid">Send</button>
		</p>
	</form>
</template>

<style type="text/css">
	.fld-error .msg-error  {
		display: block;
	}

	.msg-error {
		display: none;
	}
</style>

<script type="text/javascript">

	import axios from 'axios'
	import { required, minLength } from 'vuelidate/lib/validators'

	export default {

		data() {

			return {
				form: {
					name: '',
					job: ''
				}
			}
			
		},

		validations: {
			form: {
				name: {
					required,
					minLength: minLength( 4 )
				},
				job: {
					required
				}
			}
		},

		methods: {
			submitForm() {

				let contactFormData = new FormData();

				contactFormData.set( 'name', this.form.name );
				contactFormData.set( 'job', this.form.job );

				console.log( 'submitting data...' );

				axios( {
					method: 'post',
					url: 'https://reqres.in/api/users',
					data: contactFormData
				} ).then( function ( response ) {

					// Handle success.
					
					console.log( response );

				} ).catch( function ( response ) {

					// Handle error.
					
					console.log( response );

				} );

			}
		}

	}

</script>