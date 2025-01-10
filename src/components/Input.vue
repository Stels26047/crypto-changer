<script>
    export default{
        props:{
            changeAmount:{
                type: Function,
                required: true
            },
            convert:{
                type: Function,
                required: true
            },
            favorite:{
                type:Function,
                required: true
            }
        }
    }
</script>

<template>
  <input type="number" @input="changeAmount($event.target.value)" placeholder="Введите число"><br>
  <button @click="convert()">Конвертировать</button>
  <button @click="favorite()" className="fav-button">В избранное</button>
</template>

<style scoped>
    input{
        border-radius: 3px;
        border: 0;
        padding:  10px 15px;
        background: #fafafa;
        color: #333;
        outline: none;
        width: 500px;
        position: relative;
        top:-50px;
        font-size: 1.2em;
    }

    button{
        position: relative;
        top: -20px;
        padding: 15px 20px;
        color: #fff;
        text-transform: uppercase;
        cursor: pointer;
        background: #1a032d;
        border: 0;
        border-radius: 3px;
        transition: transform 500ms ease;
    }

    button:hover{
        transform:scale(1.05,1);
    }

    button.fav-button{
        margin-left: 20px;
    }

</style>
