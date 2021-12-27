<template>
    <div>
        <div class="rounded-t-xl overflow-hidden bg-gradient-to-r from-50 to-teal-150 p-10">
            <div class="flex justify-between">
                <div class="p-2">
                    <p class="font-bold">Sample CRUD Vue-Tailwind</p>
                </div>
                <div class="p-2">
                    <button @click="addModal = true" class="h-8 px-4 m-2 text-sm text-blue-100 transition-colors duration-150 bg-blue-500 rounded-lg focus:shadow-outline hover:bg-blue-800">
                    Add
                    </button>
                </div>
            </div>
            <div class="flex content-center">
                <table class="w-full text-center">
                    <thead>
                    <tr>
                        <th class="px-4 py-2 text-600">Id</th>
                        <th class="px-4 py-2 text-600">Name</th>
                        <th class="px-4 py-2 text-600">Age</th>
                        <th class="px-4 py-2 text-600">Created At</th>
                        <th class="px-4 py-2 text-600">Actions</th>
                    </tr>
                    </thead>
                    <tbody>
                        <tr v-for="user in users" :key="user.id">
                            <td class="border border-500 px-4 py-2 text-600 font-medium">{{user.id}}</td>
                            <td class="border border-500 px-4 py-2 text-600 font-medium">{{user.name}}</td>
                            <td class="border border-500 px-4 py-2 text-600 font-medium">{{user.age}}</td>
                            <td class="border border-500 px-4 py-2 text-600 font-medium">{{user.createdAt | moment("dddd, MMMM Do YYYY")}}</td>
                            <td class="border border-500 px-4 py-2 text-600 font-medium">
                                <button @click="viewModal = true;currentUser = user" class="h-8 px-4 m-2 text-sm text-green-100 transition-colors duration-150 bg-green-700 rounded-lg focus:shadow-outline hover:bg-green-800">
                                View
                                </button>
                                <button @click="editModal = true;currentUser = user" class="h-8 px-4 m-2 text-sm text-purple-100 transition-colors duration-150 bg-purple-700 rounded-lg focus:shadow-outline hover:bg-purple-800">
                                Edit
                                </button>
                                <button @click="deleteModal = true;currentUser = user" class="h-8 px-4 m-2 text-sm text-red-100 transition-colors duration-150 bg-red-700 rounded-lg focus:shadow-outline hover:bg-red-800">
                                Delete
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- add user modal -->
        <add-modal v-if="addModal" @close="addModal = false"></add-modal>
        <!-- view user details modal -->
        <user-modal v-if="viewModal" :current-user="currentUser" @close="viewModal = false"></user-modal>
        <!-- edit user modal -->
        <edit-modal v-if="editModal" v-model="currentUser" @close="editModal = false"></edit-modal>
        <!-- delete user modal -->
        <delete-modal v-if="deleteModal" :id="currentUser.id" @close="deleteModal = false"></delete-modal>
    </div>
</template>

<script>
import AddModal from './AddModal.vue';
import DeleteModal from './DeleteModal.vue';
import EditModal from './EditModal.vue';
import UserModal from './UserModal.vue';
export default {
  components: { EditModal, UserModal, DeleteModal, AddModal },
    data(){
        return{
            users:[],
            addModal: false,
            viewModal: false,
            editModal: false,
            deleteModal: false,
            currentUser: '',
        }
    },
    mounted(){
        //emit event for table refresh
        this.$root.$on('refresh-table', () => {
            this.fetchUsers();
        })
    },
    created(){
        //added this to created instead of mounted for faster load.
        this.fetchUsers();
    },
    methods:{
        //fetch users api call
        fetchUsers(){
            fetch(`${process.env.VUE_APP_ROOT_API}/users`)
                .then(response => response.json())
                .then(data => this.users = data);
        },
    }
}
</script>

<style>

</style>