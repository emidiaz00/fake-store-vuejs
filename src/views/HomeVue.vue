<template>
    <div>
        <div classname="background-img" v-for="item in items" :key="item.id">
            <ul>
                <li>{{ item.title }}</li>
                <li>{{ item.price }}</li>
            </ul>
        </div>
    </div>
    
    
</template>

<script>
export default {
    data() {
        return {
            items: [],
        };
    },
    created() {
        this.fetchGetProducts();
    },
    methods: {
        async fetchGetProducts() {
            const url = "https://fakestoreapi.com/products";
            const response = await fetch(url);
            try {
                this.handleResponse(response)
            } catch (error) {
                this.error = error.message;
                
            }
        },
        async handleResponse(response) {
            if (response.status >= 200 && response.status < 300) {
                // fetch api data into json
                const data = await response.json();
                this.items = data;
            } else {
                if (response.status === 404) {
                    throw new Error("Fail Fetching with this URL!");
                }
                if (response.status === 401) {
                    throw new Error("Not Authorized!");
                }
                if (response.status > 500) {
                    throw new Error("Server not Found!");
                }
            }
        },
    }
    
}
</script>

<style>
li {
    list-style: none;
}

</style>