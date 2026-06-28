// Elementos da página
const campoSenha = document.querySelector("#campo-senha");
const numeroSenha = document.querySelector(".parametro-senha__texto");
const botoes = document.querySelectorAll(".parametro-senha__botao");

const chkMaiuscula = document.querySelector("input[name='maiusculo']");
const chkMinuscula = document.querySelector("input[name='minusculo']");
const chkNumero = document.querySelector("input[name='numero']");
const chkSimbolo = document.querySelector("input[name='simbolo']");

const forcaSenha = document.querySelector(".forca");
const valorEntropia = document.querySelector(".entropia");

// Conjuntos de caracteres
const letrasMaiusculas = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
const letrasMinusculas = "abcdefghijklmnopqrstuvwxyz";
const numeros = "0123456789";
const simbolos = "!@#$%&*()_+-=[]{}<>?";

// Valor inicial
let tamanhoSenha = 12;
numeroSenha.textContent = tamanhoSenha;

// Eventos dos botões
botoes[0].onclick = diminuirTamanho;
botoes[1].onclick = aumentarTamanho;

// Eventos dos checkboxes
chkMaiuscula.onchange = geraSenha;
chkMinuscula.onchange = geraSenha;
chkNumero.onchange = geraSenha;
chkSimbolo.onchange = geraSenha;

// Gera senha ao abrir a página
geraSenha();

function diminuirTamanho() {
    if (tamanhoSenha > 1) {
        tamanhoSenha--;
        numeroSenha.textContent = tamanhoSenha;
        geraSenha();
    }
}

function aumentarTamanho() {
    if (tamanhoSenha < 20) {
        tamanhoSenha++;
        numeroSenha.textContent = tamanhoSenha;
        geraSenha();
    }
}

function geraSenha() {

    let alfabeto = "";

    if (chkMaiuscula.checked)
        alfabeto += letrasMaiusculas;

    if (chkMinuscula.checked)
        alfabeto += letrasMinusculas;

    if (chkNumero.checked)
        alfabeto += numeros;

    if (chkSimbolo.checked)
        alfabeto += simbolos;

    // Garante que exista pelo menos um conjunto selecionado
    if (alfabeto.length === 0) {
        campoSenha.value = "";
        return;
    }

    let senha = "";

    for (let i = 0; i < tamanhoSenha; i++) {
        let indice = Math.floor(Math.random() * alfabeto.length);
        senha += alfabeto[indice];
    }

    campoSenha.value = senha;

    classificaSenha(alfabeto.length);
}

function classificaSenha(tamanhoAlfabeto) {

    let entropia = tamanhoSenha * Math.log2(tamanhoAlfabeto);

    forcaSenha.classList.remove("fraca", "media", "forte");

    if (entropia > 57) {
        forcaSenha.classList.add("forte");
    } else if (entropia > 35) {
        forcaSenha.classList.add("media");
    } else {
        forcaSenha.classList.add("fraca");
    }

    let dias = (2 ** Math.floor(entropia)) / (100e6 * 60 * 60 * 24);

    valorEntropia.textContent =
        `Um computador levaria aproximadamente ${Math.floor(dias).toLocaleString()} dias para quebrar esta senha.`;
}
