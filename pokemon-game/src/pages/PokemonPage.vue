<script lang="ts">

    import PokemonOptions from "@/components/PokemonOptions.vue"
    import PokemonPicture from "@/components/PokemonPicture.vue"
    import getPokemonOptions  from "@/helpers/getPokemonOptions"
    import type { ListPokemonOptions } from "@/helpers/interfaces/listPokemonOptions.interface"

    console.log(getPokemonOptions())

   export default {
        components: { PokemonOptions, PokemonPicture },
        data(){
            return{
                pokemonArr: [] as ListPokemonOptions[],
                pokemon: null as ListPokemonOptions | null,
                showPokemon: false,
                showAnswer: false,
                message: '',
            }
        },
        methods: {
            async mixPokemonArr(){
                this.pokemonArr = await getPokemonOptions()

                const rndInt = Math.floor( Math.random() * 4)

                this.pokemon = this.pokemonArr[rndInt]

            },

            checkAnswer(selectedId: number){
                this.showPokemon = true;
                this.showAnswer = true;
                this.pokemonArr = [];

                if(!this.pokemon) return;

                if(selectedId === this.pokemon.id){
                    this.message= `Correcto, ${this.pokemon.name}`
                } else {
                    this.message = `Oopss, era ${this.pokemon.name}`
                }
            },

            newGame(){
                this.showPokemon = false;
                this.showAnswer = false;  
                this.pokemon = null;
                this.mixPokemonArr();
            }
            
            
        },

        mounted(){
          this.mixPokemonArr()  
        }

    }

</script>


<template>

    <h1 v-if="!pokemon" class=""> "Espere porfavor "</h1>

    <div v-else="pokemon" class="">
        <h1>¿Quien es este pokemon?</h1>

        <PokemonPicture :pokemonId= "pokemon.id" :showPokemon="showPokemon" />
        <PokemonOptions :pokemons ="pokemonArr" @selection="checkAnswer($event)"/>
        
        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">"Nuevo Juego"</button>
        </template>
    </div>
</template>

<style>

</style>