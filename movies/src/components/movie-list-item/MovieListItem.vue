<template>
    <li class="list-group-item d-flex justify-content-between" :class='[{ like: movie.like },
    { favourite: movie.favourite }]'>
        <span @click="$emit('onToggle', {
            id: movie.id, prop: 'like'
        })" class="list-group-item-label">
            {{ movie.name }}</span>
        <input type="number" class="list-group-item-label" :value="movie.views" />
        <div class="d-flex justify-content-center align-items-center">
            <button class="btn-cookie btn-sm" @click="$emit('onToggle', {
                id: movie.id, prop: 'favourite'
            })">
                <i class=" fas fa-cookie"></i>
            </button>
            <button class="btn-trash btn-sm" @click="$emit('onRemove', movie.id)">
                <i class="fas fa-trash"></i>
            </button>
            <i class="fas fa-star"></i>
        </div>
    </li>
</template>

<script>
export default {
    props: {
        movie: {
            type: Object,
            required: true
        }
    },
}
</script>

<style scoped>
.list-group-item {
    padding: 15px 20px;
    border: none;
    border-bottom: 1px solid #3d5a80;
}

.list-group-item:last-child {
    border-bottom: none;
}

.list-group-item span {
    line-height: 35px;
    cursor: pointer;
    font-size: 20px;
    width: 550px;
}

.list-group-item input {
    line-height: 35px;
    font-size: 20px;
    text-align: center;
    border: none;
    outline: none;
}

.list-group-item button {
    width: 35px;
    height: 35px;
    margin: 3px;
    font-size: 17px;
    border: none;
    cursor: pointer;
}

.btn-cookie {
    color: #e09f3e;
}

.btn-trash {
    color: #e5383b;
}

.list-group-item .fa-star {
    width: 35px;
    height: 35px;
    text-align: center;
    line-height: 35px;
    font-size: 16px;
    color: #fdd700;
    transition: all .3s;
    transform: translateX(30px);
    opacity: 0;
}

.list-group-item.like .fa-star {
    transform: translateX(0px);
    opacity: 1;
}

.list-group-item.favourite .list-group-item-label {
    color: #e09f3e;
}
</style>