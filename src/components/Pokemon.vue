<template>
    <div id="pokemon">
        <div class="card">
            <div class="card-image">
                <figure>
                    <img :src="pokemon.sprites.front" width="200px">
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                    <div class="media-content">
                        <p class="title is-4">{{name | capitalize}}</p>
                        <b>Type: </b>
                        <span>{{pokemon.type | capitalize}}</span>
                    </div>
                </div>
                <hr>
                <div class="content">
                    <span v-for="(ability, i) in pokemon.abilities[0]" :key="i">
                        {{ability.ability.name | capitalize}}<br>
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    created: function(){
        axios.get(this.url).then(res => {
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.sprites = {}
            this.pokemon.id = res.data.id;
            this.pokemon.sprites.front = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/"+this.pokemon.id+".png";
            this.pokemon.abilities = []
            this.pokemon.abilities.push(res.data.abilities)
        });
    },
    data(){
        return {
            pokemon: {
                type: '',
                sprites: {
                    front: '',
                },
                abilities: []
            }
        }
    },
    props: {
        num : Number,
        name : String,
        url : String
    },
    filters: {
        capitalize: function(value){
            let newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    }
}
</script>

<style>
#pokemon{
    margin-bottom:10%;
}
</style>