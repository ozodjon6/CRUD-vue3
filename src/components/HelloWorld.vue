<script setup>
import {PencilIcon,TrashIcon} from '@heroicons/vue/solid';
import axios from 'axios';
import {onMounted, reactive, ref} from "vue";

const users = ref([]);
const isUpdate = ref(false);
const info = reactive({
	id: null,
	name: '',
	username: '',
	email: '',
	phone: null,
})

// GET USER

const fetchUsers = () => {
	const data = axios.get("http://localhost:3000/users").then((res) => {
		users.value = res.data
	})
}

onMounted(() => {
	fetchUsers()
})

// CREATE USER

const createUser = () => {
	axios.post("http://localhost:3000/users", info)
	.then(() => {
		window.location.reload();
		alert('saving...')
	})
}

// UPDATE USER

const editUser = (user) => {
	isUpdate.value = true
	info.id = user.id;
	info.name = user.name;
	info.username = user.username;
	info.email = user.email;
	info.phone = user.phone;
}

const updateUser = (info) => {
	axios.put("http://localhost:3000/users/" + info.id, {
		id: info.id,
		name: info.name,
		username: info.username,
		email: info.email,
		phone: info.phone
	})
	.then(() => {
		isUpdate.value = false;
		alert('update...')
		window.location.reload()
	})
}

const deleteUser = (user) => {
	try {
		if (window.confirm('Are you want to delete')) {
			axios.delete("http://localhost:3000/users/" + user.id)
				.then(() => {
					fetchUsers()
					alert('deleting...')
				})
		}
	} catch (e) {
		console.log(e)
	}
}

</script>

<template>
  <div class="greetings">
	<h1>Creating user</h1>
	  <form class="mb-10" @submit.prevent>
		  <label class="mb-4 block max-w-3xl">
			  <span class="block mb-2 font-bold">Name:</span>
			  <input v-model="info.name" class="w-full py-2 px-4 transition border rounded border-blue-300" type="text" placeholder="name">
		  </label>
		  <label class="mb-4 block max-w-3xl">
			  <span class="block mb-2 font-bold">Username:</span>
			  <input v-model="info.username" class="w-full py-2 px-4 transition border rounded border-blue-300" type="text" placeholder="username">
		  </label>
		  <label class="mb-4 block max-w-3xl">
			  <span class="block mb-2 font-bold">E-mail:</span>
			  <input v-model="info.email" class="w-full py-2 px-4 transition border rounded border-blue-300" type="email" placeholder="email">
		  </label>
		  <label class="mb-4 block max-w-3xl">
			  <span class="block mb-2 font-bold">Phone:</span>
			  <input v-model="info.phone" class="w-full py-2 px-4 transition border rounded border-blue-300" type="number" placeholder="phone">
		  </label>
		  <div class="flex items-center">
			  <button v-if="!isUpdate" @click="createUser" class="py-2 px-4 transition border rounded border-green-400 hover:bg-green-700 hover:text-white flex justify-center items-center mr-4" type="submit">Add user</button>
			  <button v-if="isUpdate" @click="updateUser(info)" class="py-2 px-4 transition border rounded border-green-400 hover:bg-green-700 hover:text-white flex justify-center items-center" type="submit">Update</button>
		  </div>
	  </form>
	  <table class="w-full text-left">
		  <thead>
		  	<tr class="border-b border-blue-300 mb-3">
				<th class="py-3">Id:</th>
				<th class="py-3">Name:</th>
				<th class="py-3">Username:</th>
				<th class="py-3">E-mail:</th>
				<th class="py-3">Phone:</th>
			</tr>
		  </thead>
		  <tbody>
		  	<tr class="space-x-2.5 border-b" v-for="(user, index) in users" :key="index">
				<td class="py-2">{{index + 1}}</td>
				<td class="py-2">{{user.name}}</td>
				<td class="py-2">{{user.username}}</td>
				<td class="py-2">{{user.email}}</td>
				<td class="py-2">{{user.phone}}</td>
				<td class="py-2">
					<div @click="editUser(user)" class="flex cursor-pointer py-2 bg-blue-500 max-w-xs text-white mr-3 rounded items-center justify-center">
						<i class="w-6 h-6 block"><PencilIcon/></i>
						<span>Edit</span>
					</div>
				</td>
				<td>
					<div @click="deleteUser(user)" class="flex py-2 bg-red-500 cursor-pointer max-w-xs text-white rounded items-center justify-center">
						<i class="w-6 h-6 block"><TrashIcon/></i>
						<span>Delete</span>
					</div>
				</td>
			</tr>
		  </tbody>
	  </table>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
