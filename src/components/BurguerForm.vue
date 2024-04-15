<template>
    <p>formulario</p>

    <Message :msg="msg" v-show="msg"/>
    <div>
        <form action="" @submit="createBurguer" id="burguer-form">
            <div class="input-container">
                <label for="nome">Nome do cliente</label>
                <input type="text" id="nome" name="nome" v-model="nome" placeholder="digite seu nome">
            </div>

            <div class="input-container">
                <label for="nome">Escolha o pão</label>
                <select name="pao" id="pao" v-model="pao">
                    <option value="">Selecione seu pão</option>
                    <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                    {{ pao.tipo }}
                    </option>
                </select>

                </div>

                <div class="input-container">
                <label for="carne">Escolha a carne</label>
                <select name="carne" id="carne" v-model="carne">
                    <option value="">Selecione o tipo de carne</option>
                    <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
                    {{ carne.tipo }}
                    </option>
                </select>

                </div>

                <div id="opicionais-container" class="input-container">
                <label for="opicionais">Selecione os opicionais</label>
                <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                    <input type="checkbox" name="opicionais" v-model="opcionais" :value="opcional.tipo">
                    <span>{{ opcional.tipo }}</span>

                </div>
                
               

                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar um burguer">

                </div>

        </form>
    </div>
</template>

<script>

import Message from './Message.vue';



export default {
    name: 'BurguerForm',
    data() {
        return {

            paes: null,
        carnes: null,
        opcionaisdata: null,
        nome: null,
        pao: null,
        carne: null,
        opcionais: [],
        
        msg: null


        }
        

    },

    components: {
        Message
    },

    methods: {
        async getIngredientes(){

            const req = await fetch("http://localhost:3000/ingredientes");

            const data = await req.json();

            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;

            console.log(data);
        },

        //funcao para criar o hambuguer

        async createBurguer(e){

            e.preventDefault();

            //para enviar o valor dos inputs
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "solicitado"


                
            }

            
            //transforma o conteudo de data em texto
            const dataJson = JSON.stringify(data);
            
            const req = await fetch("http://localhost:3000/burgers", {//acessar esse endereço para ver o arquivo json
                method: "POST",
                headers: {"Content-type":"application/json"},
                body: dataJson
            }
            );

            const res = await req.json();

            //mensagem no sistemas
            this.msg = `Pedido n ${res.id} realizado com sucesso`;

            //limpar mensagem apso alguns segundos
            setTimeout(() => this.msg = "", 3000);

            //limpar os campos
            this.nome = "";
            this.carne = "";
            this.pao = "";
            this.opcionais = "";

            
        
        }
    },

    mounted() {

        this.getIngredientes()
    },

    components: {
        Message
    }
}
</script>

<style scoped>

#burguer-form {
    max-width: 400px;
    margin: 0 auto;


}
.input-container {

    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
}

input, select {

    padding: 5px 10px;
    width: 300px;
}

#opicionais-container {
    flex-direction: row;
    flex-wrap: wrap;
}

#opicionais-title {
    width: 100%;
}

.checkbox-container {

    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input{
    width: auto;

}

.checkbox-container span{
    font-weight: bold;
    margin-left: 6px
}





.submit-btn {

    background-color: #222;
    color: orange;
    font-weight: bold;
    border: 2px solid black;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
   
    
}


.submit-btn:hover {

    background-color:  transparent;
    color: black




}
</style>