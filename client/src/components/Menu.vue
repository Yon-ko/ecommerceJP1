<template>
    <div class="ui secondary menu">
        <div class="ui container">
            <div class="left menu">
                <router-link class="item" to="/">
                <img class="ui small image" src="../assets/logo.png" alt="Ecomerce" />
                </router-link>
                <template v-for="category in categories" :key="category.id">
                <router-link class="item" :to="category.slug">
                    {{ category.title }}
                </router-link> 
                </template> 
            </div>

            <div class="right menu">
                <router-link class="item" to="/login" v-if="!token">
                inciar sesion
                </router-link>

                <template v-if="token">

                <span class="ui item cart">
                <i class="shopping cart icon" @click="openCart"></i>
                </span>
                <span class="ui item logout" @click="logout">
                <i class="sign-out icon"></i>
                </span>


                <router-link class="item" to="/orders">Pedidos</router-link>
                <span class="ui item cart">
                <i class="shopping cart icon"></i>
                </span>
                <span class="ui item logout" @click="logout">
                <i class="sign-out icon"></i>
                </span>
                </template>

            </div>


        </div>
    </div>

</template>

<script>
import { ref, onMounted } from 'vue';
import { useStore } from 'vuex';
import { getTokenApi, deleteTokenApi } from '../api/token';
import { getCategoriesApi } from '../api/category';


export default {
    name: 'Menu',

    setup() {
    let categories = ref(null);
     const token = getTokenApi();
    const store = useStore();

    onMounted( async () =>  {
        const response = await getCategoriesApi();
        categories.value= response;

    });

    // const token = getTokenApi();
    const logout = () => {
        
        deleteTokenApi();
        location.replace('/');
    };
    const openCart = () => {
      store.commit('setShowCart', true);
    };


    return {
        token,
      logout,
      categories,
      openCart,
        };
    },
};
</script>

<style lang="scss" scoped>

.ui.menu.secondary {
    
    background-color: #000000b7;

    .item {
        
        color: #9fff05;
        &:hover {
            color: #f10741;
        }
    }

    .menu.left {
       
    width: 100%;
    .ui.image {
        width: 100px;
        }
    }

.menu.rigth{
    width: 100%;
    justify-content: flex-end;

    .logout,
    .cart {
    &:hover {
    cursor: pointer;
    }
    }
    }
}
</style>