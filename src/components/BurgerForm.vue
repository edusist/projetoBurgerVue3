<template>
    <div>
        <!-- <p>Formulário de componentes do hamburger</p> -->
        <message :msg="msg" v-show="msg" />
        <div>
            <form id="burger-form" @submit="createBurger">
                <div id="input-container">
                    <label for="nome">Nome do Cliente:</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
                </div>

                <div id="input-container">
                    <label for="pao">Escolha o pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione o seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                            {{ pao.tipo }}
                        </option>
                    </select>
                </div>
                <div id="input-container">
                    <label for="carne">Escolha o carne do seu Burger:</label>
                    <select name="carne" id="carne" v-model=carne>
                        <option value="">Selecione o tipo de carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
                            {{ carne.tipo }}
                        </option>
                    </select>
                </div>

                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.id">
                        <input type="checkbox" name="opcionais" id="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>

                </div>
                <div id="input-container">
                    <input type="submit" value="Criar meu Burger" class="submit-btn">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue';


export default {
    name: 'BurgerForm',

    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisData: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            status: null,
            msg: null
        }
    },

    methods: {
        //Trazer os ingredientes do BD Json
        async getIngredientes() {
            //Função fetch me promete entregar alguma coisa
            const requisacao = await fetch('http://localhost:3000/ingredientes');//Endpoint e url final 

            //await(espera) recebe uma Promise e a transforma em um valor de retorno  (ou lança uma exceção em caso de erro).
            const data = await requisacao.json();
            // console.log(data);

            //Dados são preenchidos vindo do servidor
            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisData = data.opcionais;
        },
        // Adicionar os itens no bd
        async createBurger(e) {

            e.preventDefault();
            // console.log('Criou um hamburger');
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: 'Solicitado',
            }
            //Transformar os dados no string Json
            const dataJson = JSON.stringify(data);
            //Fetch vai na API e traz tal resultado, tenho que informar qual url quero pegar o resultado
            // await -> retorne uma promessa
            //Tenho adicionar burger em array vazio no BD 
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" }, //Comunicação feita por JSON
                body: dataJson

            });

            //Espero uma resposta
            const res = await req.json();
            // console.log(res);

            //Mensagem do sistema
            this.msg = `Pedido Nº ${res.id} realizado com sucesso`
            //Limpar mensagem
            setTimeout(() => this.msg = "", 3000);

            //Limpa os campos 
            this.nome = '';
            this.carne = '';
            this.pao = '';
            this.opcionais = '';
            // console.log(data );
        }
    },
    //Quanto inicializar a página carrega a função getIngredientes 
    // LifeCycle Hooks
    mounted() {
        this.getIngredientes();
    },
    components: {
        Message
    }
}

</script>

<style scoped>
#burger-form {
    max-width: 400px;
    margin: 0 auto;
}

#input-container {
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

input,
select {
    padding: 5px 10px;
    width: 300px;
}

#opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
}

#opcionais-title {
    width: 100%;
}

.checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
    margin-top: 20px;

}

.checkbox-container span,
.checkbox-container input {
    width: auto;
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold;

}

.submit-btn {
    background-color: #222;
    color: #fcba03;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.submit-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>