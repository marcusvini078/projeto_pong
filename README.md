# projeto_pong
projeto desenvolvido na disciplina de pensamento computacional
//variáveis da bolinha
let xBolinha = 300;
let yBolinha = 200; 
let diametro = 15;
let raio = diametro / 2;

//velocidade da bolinha
let velocidadeXBolinha = 6;
let velocidadeYBolinha = 6; 

//variáveis da raquete
let xRaquete = 5;
let yRaquete = 150; 
let raqueteComprimento = 10;
let raqueteAltura = 90;

//variáveis do oponente
let xRaqueteOponente = 585; 
let yRaqueteOponente = 150;
let velocidadeYOponente;

let colidiu = false; 

//placar do jogo
let meusPontos = 0;
let pontosDoOponente = 0;

//sons do jogo
let raquetada; 
let ponto; 
let trilha; 

function preload(){
  trilha = loadSound("trilha.mp3");
  ponto = loadSound("raquetada.mp3");
  raquetada = loadSound("ponto.mp3");
}

function setup() {
  createCanvas(600, 400);
  trilha.loop();
}

function draw() {
  background(32,178,170);
  stroke(51);
  rect(298.5, 32, 3, 336);
  mostraBolinha();
  movimentaBolinha();
  verificaColisaoBorda();
  mostraRaquete(xRaquete,yRaquete);
  movimentaMinhaRaquete();
  verificaColisaoRaquete(xRaquete, yRaquete);
  mostraRaquete(xRaqueteOponente, yRaqueteOponente);
  movimentaRaqueteOponente();
  verificaColisaoRaquete(xRaqueteOponente, yRaqueteOponente);
  incluiPlacar(); 
  marcaPonto();
}

function mostraBolinha(){
  circle(xBolinha, yBolinha, diametro);
  stroke(255);
  fill(color(79,79,79));
}

function movimentaBolinha(){
  xBolinha += velocidadeXBolinha;
  yBolinha += velocidadeYBolinha;
}

function verificaColisaoBorda(){
  if(xBolinha + raio > width || 
    xBolinha - raio < 0){
    velocidadeXBolinha *= -1;

  }

  if(yBolinha + raio > height || yBolinha - raio < 0){
    velocidadeYBolinha *= -1;
  }
}

function mostraRaquete(x, y){
  rect(x, y, raqueteComprimento, raqueteAltura);
}

function movimentaMinhaRaquete(){
  if(keyIsDown(UP_ARROW)){
    yRaquete -= 10; 
  }
  if(keyIsDown(DOWN_ARROW)){
    yRaquete += 10; 
  }
}

function verificaColisaoRaquete () {
  if(xBolinha - raio < xRaquete + raqueteComprimento && yBolinha - raio < yRaquete + raqueteAltura && yBolinha + raio > yRaquete) {
    velocidadeXBolinha *= -1; 
    raquetada.play();
  }
}

function verificaColisaoRaquete(x, y){
  colidiu = collideRectCircle(x, y, raqueteComprimento, raqueteAltura, xBolinha, yBolinha, raio);
  if(colidiu){
    velocidadeXBolinha *= -1;
    raquetada.play();
  }
}

function movimentaRaqueteOponente(){
  velocidadeYOponente = yBolinha - yRaqueteOponente - raqueteComprimento / 2 - 30;
  yRaqueteOponente += velocidadeYOponente; 
}

function incluiPlacar(){
  stroke(255);
  textAlign(CENTER);
  textSize(18);
  fill(color(255,140,0));
  rect(150, 10, 40, 20);
  fill(255);
  text(meusPontos, 170, 26);
  fill(color(250,140,0));
  rect(450, 10, 40, 20);
  fill(255);
  text(pontosDoOponente, 470,26);
