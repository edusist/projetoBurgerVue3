<template>
    <div>
        <!-- <p>Formulário de componentes do hamburger</p> -->
        <p>Componente de mensagem</p>
        <div>
            <form  id="burger-form">
                <div id="input-container">
                    <label for="nome">Nome do Cliente:</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
                </div>

                <div id="input-container">
                    <label for="pao">Escolha o pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione o seu pão</option>
                        <option value="integral">Integral</option>
                    </select>
                </div>  
                     <div id="input-container">
                    <label for="carne">Escolha o carne do seu Burger:</label>
                    <select name="carne" id="carne" v-model=carne>
                        <option value="">Selecione o tipo de carne</option>
                        <option value="maminha">Maminha</option>
                    </select>
                </div>  

                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
                        <div class="checkbox-container">
                            <input type="checkbox" name="opcionais" id="opcionais" v-model="opcionais" value="salame">
                            <span>Salame</span>
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


export default {
    name: 'BurgerForm'
}
</script>

<style scoped>
    #burger-form{
        max-width: 400px;
        margin: 0 auto;
    }

    #input-container{
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label{
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #fcba03;
    }
    input, select{
        padding: 5px 10px;
        width: 300px;
    }

    #opcionais-container{
        flex-direction: row;
        flex-wrap: wrap;
    }
    #opcionais-title{
        
    }
</style>