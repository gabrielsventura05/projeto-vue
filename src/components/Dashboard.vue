<template>
    
   
    <div id="burger-table">

        <Message :msg="msg" v-show="msg"/>
  
        <div>
        <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opicionais:</div>
        <div>Ações: </div>


    </div>
</div>

        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id" >
                <div class="order-number">{{ burger.id }}</div>
                    <div>{{ burger.nome }}</div>
                    <div>{{ burger.pao }}</div>

                    <div>{{ burger.carne }}</div>

                    <div>
                        <ul>
                        <li v-for="(opcional, index) in burger.opcionais" :key="index">
                          {{ opcional }}  </li>
                        
                    </ul>
                    </div>
                    <div>
                        <select name="status" id="status" @change="updateBurger($event, burger.id)">
                            <option value="">Selecione</option>
                            <!--aqui selected recebe o tipo do status mesmo depois que a página for atualizada-->
                            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">
                            {{ s.tipo }}
                            </option>
                        </select>
                        <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                    </div>

            </div>

        </div>
        
    </div>

    


</template>

<script>

import Message from './Message.vue';

export default {

    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        }
    },

    components: {
        Message
    }, 
    methods: {

        //funcao assincrona
        async getPedidos() {

            const req = await fetch("http://localhost:3000/burgers");

            const data = await req.json();

            this.burgers = data;

            console.log(this.burgers);

            this.getStatus();
            
        },

        async getStatus() {

            const req = await fetch("http://localhost:3000/status");

            const data = await req.json();

            this.status = data;

            
        },

        async deleteBurger(id){

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {

                method: "DELETE"
            });

            const res = await req.json();

             //mensagem no sistemas
            this.msg = `Pedido removido com sucesso`;

            //limpar mensagem apso alguns segundos
            setTimeout(() => this.msg = "", 3000);


            this.getPedidos();
           
        },

        async updateBurger(event, id) {

            const option = event.target.value;

            const dataJson = JSON.stringify({status: option});//coloca o status em string

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {

                method: "PATCH", //atualiza somente o que foi enviado, no caso, o status
                headers: {"Content-type": "application/json"},
                body: dataJson

            });

            const res = await req.json();

             //mensagem no sistemas
             this.msg = ` o Pedido n ${res.id} foi atualizado para ${res.status}`;

//limpar mensagem apso alguns segundos
setTimeout(() => this.msg = "", 3000);


            console.log(res);


            


        }
    },



    //para chamar um metodo
    mounted() {
        this.getPedidos();
    }
}
</script>



<style>

#burger-table {

    max-width: 1200px;
    margin: 0px auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row{

    display: flex;
    flex-wrap: wrap;
}

#burger-table-heading {

    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

#burger-table-heading div,
.burger-table-row div {
    width: 19%;
}

.burger-table-row {

    width: 100%;
    padding: 12px;
    border: 1px solid #ccc;
}

#burger-table-heading .order-id,
.burger-table-row .order-number
{
    width: 5%;
}

select{
    padding: 12px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: orange;
    font-weight: bold;
    border: 2px solid black;
    padding: 10px;
    margin: 0 AUTO;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}

</style>
