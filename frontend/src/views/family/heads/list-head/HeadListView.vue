<script lang="ts">
import Services from '@/services/Services';
export default {
    name: 'UserListView',
    data() {
        return {
            users: [],
            thisvalue: [],
        };
    },
    mounted() {
        this.listAllUsers()
    },
    methods: {
        async listAllUsers() {
            try {
                const response = await Services.Users.getUsresAddressAll()
                console.log(response);
                
                this.users = response;
            } catch (error) {
                console.error('Error: ', error);
            }
        },
        async removerUser(id: number){
            const response = await Services.Users.deleteUser(id)
            console.log('delete');
            this.listAllUsers()
        },
        value(id: number){
            return true
        }
    }
}
</script>
<template>
   <header class="flex my-4 w-full flex-col lg:flex-row">
        <h1 class="text-5xl basis-10/12">Famílias</h1>
        <router-link to="/family/create" class="flex-1 w-3/6 bg-sky-500 rounded-xl text-center flex justify-evenly lg:m-0 mt-4">
            <iconify-icon icon="material-symbols:add-2-rounded" width="35" height="35"
                class="self-center text-center text-slate-200"></iconify-icon>
            <span class="text-slate-200 text-lg self-center leading-none">Adicionar Família</span>
        </router-link>
    </header>
    <hr class="mb-7">
    <main class="flex flex-col">
        <!--Divisão para cada usuários-->
        <section v-for="user in users" :key="user.users.id" class="m-1">
        <div class="p-2 bg-gray-200 border-slate-500 rounded-lg border-2 dark:border-slate-400 dark:bg-gray-600" v-for="user in users"
            :key="user.users.id">
            <header class="flex flex-row min-w-min">
                <div class="flex-1 self-center text-center" v-if="value(user.users.id)">
                    <div class="" @click="value(user.users.id)">
                        <iconify-icon icon="line-md:chevron-down-circle-twotone" width="30" height="30"></iconify-icon>
                    </div>
                </div>
                <div class="flex-1 self-center text-center" v-else>
                    <div class="lg:w-28 w-14 bg-red-700 text-red-950 rounded-lg">
                        <b class="py-0.5">Inativo</b>
                    </div>
                </div>
                <div class="basis-10/12 lg:text-4xl text-xl pl-6">{{ user.users.name }}
                </div>
                <!--Fim Contatos-->
                <div @click="blockUser(user.users.id)" class="self-center px-1 hover:cursor-pointer ">
                    <iconify-icon icon="material-symbols:lock" width="30" height="30"  class="text-sky-600 hover:text-sky-700" data-tooltip="Ativar/Inativar usuário"></iconify-icon>
                </div>
                <div class="flex-1 self-center">
                    <router-link :to="{ name: 'Atualizar Usuário', params: { id: user.users.id } }" >
                        <iconify-icon icon="material-symbols:edit-square-outline-rounded" width="30" height="30" data-tooltip="Editar usuário"
                            class="text-yellow-500"></iconify-icon>
                    </router-link>
                </div>
                <div class="flex-1 self-center">
                    <button @click="removerUser(user.users.id)">
                        <iconify-icon icon="material-symbols:delete-rounded" width="30" height="30" data-tooltip="Excluir usuário"
                            class="text-red-600"></iconify-icon>
                    </button>
                </div>
            </header>
            <!-- <hr> -->
            <!-- <section class="flex flex-col min-w-min basis-7/12 self-center"> -->
            <!--Contatos-->
            <!-- <div class="flex-1">Email: {{ user.users.email }}</div>
                <div class="flex-1">Telefone: {{ user.users.cellphone }}</div>
                <div class="flex-1">Função: {{ user.users.privileges }}</div> -->
            <!--Fim Contatos-->
            <!-- </section> -->
        </div>
        <div class="bg-black h-32 ">

        </div>
    </section>
    </main>
</template>
