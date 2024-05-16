<template>
    <div id="burger-table">
        <message :msg="msg" v-show="msg" />
        <div id="burger-table-heading">
            <div class="order-id">#:</div>
            <div>Cliente:</div>
            <div>Pão:</div>
            <div>Carne:</div>
            <div>Opcionais:</div>
            <div>Ações:</div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.nome }}</div>
                <div>{{ burger.pao }}</div>
                <div>{{ burger.carne }}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in burger.opcionais" :key="index">
                            {{ opcional }}
                        </li>

                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurger($event, burger.id)">
                        <option value="">Selecione</option>
                        <option v-for="stt in status" :key="stt.id" :value="stt.tipo" :selected="burger.status == stt.tipo">
                            {{ stt.tipo }}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
import Message from "./Message.vue"
export default {
    name: 'Dashboard',

    components: {
        Message
    },

    data() {
        return {
            burgers: null,
            burgers_id: null,
            status: [],
            msg: null
        }
    },

    methods: {   //Todas as Funções

        //Função Assincrona  traz os Pedidos
        async getPedidos() {

            //await uma promessa 
            const req = await fetch('http://localhost:3000/burgers');
            const data = await req.json();

            //O que vier de data guardado na variavel burgers
            this.burgers = data;

            // console.log(this.burgers);

            //Resgatar o status
            this.getStatus();
        },

        //Função Assincrona traz os status 
        async getStatus() {
            const req = await fetch('http://localhost:3000/status');
            const data = await req.json();

            this.status = data;
        },

        //Função Assincrona Deleta Hamburgue
        async deleteBurger(id) {

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'DELETE',

            });

            const res = await req.json();

            //Mensagem do sistema
            this.msg = `Pedido removido com sucesso!`;
            //Limpar mensagem
            setTimeout(() => this.msg = "", 3000);

            //Atualiza os dados lista pedidos
            this.getPedidos();

        },

        //Função Assincrona atualizar Hamburguer
        async updateBurger(event, id) {

            //Vai pegar o valor que usuário digitou 
            //event.target -> Obtenha o elemento onde o evento ocorreu
            //event.target.value ->Obtenha o valor do elemento
            const option = event.target.value;

            const dataJson = JSON.stringify({ status: option });

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                //PATCH -> Atualiza somente o que foi enviado no caso o status, as outras informações não serão alteradas
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json' },//Comunicação feita por JSON  
                body: dataJson

            });

            const res = await req.json();//Depois vai ser obtida a resposta 
            // console.log(res);

            //Mensagem do sistema
            this.msg = `O Pedido Nº ${res.id} foi alterado para ${res.status}!`;
            //Limpar mensagem
            setTimeout(() => this.msg = "", 3000);

            //Atualiza os dados lista pedidos
            this.getPedidos();
        }

    },
    mounted() {
        this.getPedidos();

    }
}
</script>

<style scoped>
#burger-table {
    max-width: 1200px;
    margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
    display: flex;
    flex-wrap: wrap;



}

#burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

.burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;

}

#burger-table-heading div,
.burger-table-row div {
    width: 19%;

}

#burger-table-heading .order-id,
.burger-table-row .order-number {
    width: 5%;
}

select {
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>