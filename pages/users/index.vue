<template>
    <div>
        <h1>用户列表</h1>
        <ul>
            <li><nuxt-link :to="{name:'users-id',params:{id:111}}">user 1-1</nuxt-link></li>
            <li><nuxt-link :to="{name:'users-id',params:{id:222}}">user 2-1</nuxt-link></li>
        </ul>
    </div>
</template>

<script>
export default {
    
}
</script>