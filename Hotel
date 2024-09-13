/* 06/07/2023

O programa simula um sistema de hotel, em que são representadas a alocação 
dos apartamentos e a ocupação dos quartos do hotel a partir do mapa. As funções 
permitem ao usuário fazer reserva e check-in de um apartamento, e também o 
check-in no hotel a partir de uma reserva feita anteriormente. Ainda, é possivel 
cancelar a reserva e realizar o check-out.*/

#include <stdio.h> //declarações dos protótipos das funções da biblioteca de E/S padrão
#include <stdlib.h> //declarações dos protótipos das funções da biblioteca padrão
#include <string.h> //declarações dos protótipos das funções da biblioteca string
#define MAX_NOME 40 //número máximo de caracteres que o nome do cliente pode ter e seu endereco de e-mail
#define N_ID 12 //número máximo de dígitos do documento de identificação do cliente, por exemplo, o cpf(11 dígitos) no Brasil, (sem traço ou ponto)
#define LIN 20 //número total de linhas(andares) do hotel
#define COL 14 //número total de colunas(quartos por cada andar) do hotel
#define N_NUM 15 //número máximo de dígitos do telefone
#define N_CEP 8 //número máximo de dígitos do CEP
/*
No mapa do Hotel,
'.' significa vazio
'R' significa reservado
'O' significa ocupado
*/

struct stHotel //estrutura para os dados do cliente e para a matrix do hotel
{
	char nome [MAX_NOME]; //nome do cliente
    char id[N_ID]; //cpf do cliente ou documento de identificação única
    char telefone[N_NUM]; //número de telefone do cliente
    char mail[MAX_NOME]; //e-mail do cliente
    long int cep; //cep do cliente
	int qtd_pessoas; //quantidade de pessoas que ficarão no quarto
    char status; // pode ser 'R' 'O' ou '.'
};
//declarações dos protótipos das funções utilizadas
void LimpaEntrada (void); //USAR DEPOIS DE TODO SCANF - limpa o buffer
void fPreencheHotel (struct stHotel h[LIN][COL]); //função que gera o mapa do hotel
void fMostraHotel(struct stHotel h[LIN][COL]); //função que imprime/mostra o mapa do hotel
int fVoltar(int i_mud, int j_mud); //função para sair e voltar para o menu principal. Se o usuário inserir "-1", sai.
void fFazerCadastro (struct stHotel h[LIN][COL]); //função para realizar cadastro, onde o usuário deve colocar o número do andar e apartamento desejado e o ID e informações pessoais do cliente
void fMostraCadastro(int i_mud, int j_mud, struct stHotel h[LIN][COL]);//mostra os dados do cliente e o status do quarto

int fFazerReserva(int i_mud, int j_mud, struct stHotel h[LIN][COL]); //reserva o quarto se estiver disponível (vazio)
int fID_Verificar(int i_mud, int j_mud, struct stHotel h[LIN][COL]);
void fCheckInComReserva(struct stHotel h[LIN][COL]); //faz o check-in de um quarto reservado
int fCheckInSemReserva(int i_mud, int j_mud, struct stHotel h[LIN][COL]); //faz o check-in de um quarto vazio

int fCancelarReserva(struct stHotel h[LIN][COL]); //cancela uma reserva e deixa o quarto vazio
int fCheckOut(struct stHotel h[LIN][COL]); //desocupa um quarto, o deixando vazio

int fQuartoValido(int i_mud, int j_mud); //verifica se o quarto é valido
int fQuartoReservado(int i_mud, int j_mud, struct stHotel h[LIN][COL]); //verifica se o quarto está reservado (terá a letra "R" no mapa)
int fQuartoOcupado(int i_mud, int j_mud, struct stHotel h[LIN][COL]); //verifica se o quarto está ocupado (terá a letra "O" no mapa)
int fQuartoVazio(int i_mud, int j_mud, struct stHotel h[LIN][COL]);//verifica se o quarto está reservado (terá  "." no mapa)

void fMenuTaxa(struct stHotel h[LIN][COL]); //mostra o menu específico para as taxas do hotel
void fTaxaReserva(struct stHotel h[LIN][COL]); //mostra a taxa de reservas do hotel
void fTaxaOcupado(struct stHotel h[LIN][COL]); //mostra a taxa de ocupação do hotel
void fTaxaVazio(struct stHotel h[LIN][COL]); //mostra a taxa de quartos vazios do hotel

void fVerificarStatus(struct stHotel h[LIN][COL]); //verifica o statsu de um quarto
void fMostraMenu(void); //mostra o menu principal



void LimpaEntrada (void) { //limpa o buffer
    char carac;
	while ((carac = fgetc(stdin)) != EOF && carac != '\n')
		;
} //função dada pelo professor

void fPreencheHotel(struct stHotel h[LIN][COL]){ //preenche a matriz Hotel com ponto
    int i, j;
   	for (i = 0; i < LIN; i++)
		for (j = 0; j < COL; j++)
			h[i][j].status = '.';
} //declara dois contadores, um para linha (andar) e um para a coluna (apto)

void fMostraHotel(struct stHotel h[LIN][COL]){ //mostra na tela a matriz Hotel
    int i, j;
    printf ("\n\nApartamento");
	for (j = 0; j <= COL-1; j++)
		printf ("\t%d", j+1);
		
	for (i = LIN-1; i >= 0; i--){
		printf ("\nAndar %2d", i+1);
		for (j = 0; j < COL; j++)
			printf ("\t%c  ", h[i][j].status);
	}
    printf ("\n"); //mostra a legenda
    printf ("\nLEGENDA:\n |.| - Quarto Disponivel \n |R| - Quarto Reservado \n |O| - Quarto Ocupado \n\n");
}

int fVoltar(int i_mud, int j_mud){ //verifica se o usuário quer sair da tela e voltar ao menu principal
    if ((i_mud == -1) && (j_mud == -1))
		return 1;
    else 
        return 0;
}

void fFazerCadastro (struct stHotel h[LIN][COL]){
    //faz o cadastro do cliente, pede o número do quarto e permite realizar uma reserva ou o check-in imediatamente
	int opt; //opção para selecionar entre reservar e fazer check-in
	int i_mud, j_mud; //andar e quarto digitados pelo usuário
	
	fMostraHotel(h); //mostra a situação atual do hotel
	
	printf("\nDigite o quarto de interesse.\n");
	
	do{ //pede e testa andar/apto até que sejam válidos
    printf("Para sair e voltar ao menu, digite -1 nas opcoes!\n");
    printf("Forneca o andar (entre 1 e 20): "); //(linha)
	scanf("%d", &i_mud);
	printf("\n");
    LimpaEntrada();
	printf("Forneca o numero do apartamento (entre 1 e 14): "); //(coluna)
	scanf("%d", &j_mud);
    LimpaEntrada();

    if(fVoltar(i_mud, j_mud)){ //se for verdadeiro, volta para o menu
        return;
    }

    }
    while(!fQuartoValido(i_mud, j_mud));

	printf("\nFACA O CADASTRO\n");
	
	do{
    printf("\nDigite o nome do cliente: ");
    gets(h[i_mud-1][j_mud-1].nome);

    if(strlen(h[i_mud-1][j_mud-1].nome)>MAX_NOME-1){
        printf("O tamanho do nome excedeu o tamanho maximo para este campo. \nPor Favor, digite novamente\n");
    }

    }
    while(strlen(h[i_mud-1][j_mud-1].nome)>MAX_NOME-1);

    int IdInvalido = 1;
	while (IdInvalido) {
    printf("\nDigite o documento de identificacao unica (ID) de, no maximo, 11 digitos\nNo Brasil - CPF\n(sem pontos ou tracos): ");
    gets(h[i_mud-1][j_mud-1].id);

    if (strlen(h[i_mud-1][j_mud-1].id) > N_ID-1) { //Verifica se o ID ou número de CPF é válido
        printf("\nNumero Invalido!(Digite novamente)\n");
    } 
    else {
        IdInvalido = 0; // O número do ID é válido, sair do loop
    }
    
    }
    
    printf("\nDigite o endereco(CEP): "); //endereço(cep)
    scanf("%ld",&h[i_mud-1][j_mud-1].cep);
    LimpaEntrada();
    
    int TelefoneInvalido = 1;
	while (TelefoneInvalido){
    printf("\nDigite o telefone: "); //telefone
    gets(h[i_mud-1][j_mud-1].telefone);
    
    if (strlen(h[i_mud-1][j_mud-1].telefone) > N_NUM-1) { //Verifica se o número de telefone  é válido
        printf("\nNumero Invalido!(Digite novamente)\n");
    } 
    else {
        TelefoneInvalido = 0; // O número de telefone é válido, sair do loop
    }
    
   	}
    printf("\nDigite o e-mail: "); //e-mail
    gets(h[i_mud-1][j_mud-1].mail);
    

    printf("\nQuantidade total de pessoas no quarto: "); //número de pessoas
	scanf("%d", &h[i_mud-1][j_mud-1].qtd_pessoas);
    LimpaEntrada();

    //depois de fornecer os dados do cliente e o quarto de interesse, o gerente escolhe o que fazer com o quarto
    printf("\nDeseja realizar a reserva ou o check-in?\n");
    printf("\nDigite 1 para realizar a reserva");
    printf("\nDigite 2 para realizar o check-in");
    printf("\nOpcao: ");
    scanf("%d", &opt);
    LimpaEntrada();

    switch(opt){
        case 1:
            if(fFazerReserva(i_mud, j_mud, h)){ //se a opção escolhida for 1, permite realizar a reserva de um quarto
				printf("\nCadastro realizado com sucesso!\n");
                fMostraCadastro(i_mud, j_mud, h); //mostra o cadastro do cliente
            }
			break;
        case 2:
            if(fCheckInSemReserva(i_mud, j_mud, h)){ //se a opção escolhida for 2, permite realizar a o check-in de um quarto diretamente
			    printf("\nCadastro realizado com sucesso!\n");
                fMostraCadastro(i_mud, j_mud, h); //mostra o cadastro do cliente
			}
            break;
    }
}



void fMostraCadastro(int i_mud, int j_mud, struct stHotel h[LIN][COL]){ //mostra os dados do cadastro do cliente e o status do quarto 
    printf("\nDADOS DO CLIENTE\n");
	printf ("\nNome: %s", h[i_mud-1][j_mud-1].nome); //nome
    printf("\nID: %s", h[i_mud-1][j_mud-1].id); // documento de identificação única do cliente, ex: CPF no Brasil
    printf("\nCEP: %ld", h[i_mud-1][j_mud-1].cep); //código postal
    printf("\nTelefone: %s", h[i_mud-1][j_mud-1].telefone); //telefone
    printf("\ne-mail: %s",h[i_mud-1][j_mud-1].mail); //e-mail
	printf ("\nTotal de pessoas no quarto: %d", h[i_mud-1][j_mud-1].qtd_pessoas); //qtd total de pessoas
    printf("\nAndar: %d\nQuarto: %d", i_mud, j_mud);//localização
    printf("\nStatus do quarto: %c", h[i_mud-1][j_mud-1].status);//status ('R','O','.') - de acordo com a legenda
	printf ("\n"); //mostra a legenda
    printf ("\nLEGENDA:\n |.| - Quarto Disponivel \n |R| - Quarto Reservado \n |O| - Quarto Ocupado \n\n");
}

int fFazerReserva(int i_mud, int j_mud, struct stHotel h[LIN][COL]){ //reserva um quarto
    if(fQuartoVazio(i_mud, j_mud, h)){ //se o quarto for válido e estiver vazio
        h[i_mud-1][j_mud-1].status = 'R'; //muda para reservado
        printf ("\nReserva do apartamento (%d) do andar (%d) foi realizada!\n", j_mud , i_mud);
        return 1;
    }
    else if ((fQuartoOcupado(i_mud, j_mud, h)) || (fQuartoReservado(i_mud, j_mud, h))) {
        printf("\n\nQuarto Indisponivel!\n");
        return 1;
    }
    else {
        return 0;
    }  
    
}

int fID_Verificar(int i_mud, int j_mud, struct stHotel h[LIN][COL]) //verifica o ID (CPF) do cliente
{
	char idFornecido[N_ID]; //ID fornecido pelo cliente
	printf("Digite o ID ou CPF (documento de identificacao unica): ");
	gets(idFornecido);
	
	if (strcmp (h[i_mud-1][j_mud-1].id, idFornecido) == 0) //se o ID digitado for o mesmo do ID do cliente cadastrado, retorna 1
	{
		return 1;
	}
	else
		return 0;
}

void fCheckInComReserva(struct stHotel h[LIN][COL]){ //realiza o check-in de um quarto já reservado anteriormente, sem precisar passar pelo cadastro
int i_mud, j_mud; //andar e quarto digitados pelo usuário
do{ //pede testa andar/apto até que sejam válidos
    printf("Para sair e voltar ao menu, digite -1 nas opcoes!\n");
    printf ("Forneca o andar (entre 1 e 20): "); //(linha)
	scanf ("%d", &i_mud);
	printf("\n");
    LimpaEntrada();
	printf ("Forneca o numero do apartamento (entre 1 e 14): "); //(coluna)
	scanf ("%d", &j_mud);
    LimpaEntrada();

    if(fVoltar(i_mud, j_mud)){ //se for verdadeiro, volta para o menu
        return;
    }

    }
    while(!fQuartoValido(i_mud, j_mud));

    if (fQuartoReservado(i_mud, j_mud, h)) //verifica se o quarto já foi reservado
	{
        if (fID_Verificar (i_mud, j_mud, h)) //se o id for o mesmo
        {
        	h[i_mud-1][j_mud-1].status = 'O';
        	printf("\nCheck-in do do apartamento (%d) do andar (%d) foi realizado!\n", j_mud , i_mud );
        	return; //se sim, então realiza o check-in
    	}
    	else //se não for igual, não realiza o check-in
    	{
    		printf ("\nO registro de identificacao digitado nao corresponde ao do cadastro escolhido!\n");
    		return;
		}
   	}
   	else if (fQuartoOcupado(i_mud, j_mud, h)) //se o quarto já estiver ocupado, mostra uma mensagem
		{
    		printf ("\nEste quarto ja' esta' ocupado!\n");
			return;
    	}
   	
	else //senão, significa que o quarto não está reservado
	{
   		printf("\nEste quarto nao esta' reservado!\n");
        return;
	}
        
}

int fCheckInSemReserva(int i_mud, int j_mud, struct stHotel h[LIN][COL]){
//essa função permite realizar o check-in diretamente sem ter uma reserva
    if (fQuartoVazio(i_mud, j_mud, h)){ //se o quarto estiver vazio, faz o check-in
        h[i_mud-1][j_mud-1].status = 'O'; 
        printf("\nCheck-in do apartamento (%d) do andar (%d) foi realizado!\n", j_mud , i_mud);
        return 1;
    }
    else if (fQuartoOcupado(i_mud, j_mud, h)){ //se o quarto já estiver ocupado, mostra uma mensagem
    	printf ("\nEste quarto ja' esta' ocupado!\n");
	}
	else if (fQuartoReservado(i_mud, j_mud, h)){ //se o quarto já estiver reservado, mostra uma mensagem
    	printf ("\nEste quarto ja' esta' reservado!\n");
 	}
	return 0;
}




int fCancelarReserva(struct stHotel h[LIN][COL]){ //cancela uma reserva
	int i_mud, j_mud; //andar e quarto digitados pelo usuário
	do{ //pede testa andar/apto até que sejam válidos
        printf("Para sair e voltar ao menu, digite -1 nas opcoes!\n");
        printf ("Forneca o andar (entre 1 e 20): "); //(linha)
	scanf ("%d", &i_mud);
	printf("\n");
        LimpaEntrada();
	printf ("Forneca o numero do apartamento (entre 1 e 14): "); //(coluna)
	scanf ("%d", &j_mud);
        LimpaEntrada();

        if(fVoltar(i_mud, j_mud)){ //se for verdadeiro, sai
            return 0;
        }
        }
        while(!fQuartoValido(i_mud, j_mud));
	
	if (fQuartoReservado(i_mud, j_mud, h)){ // se o quarto estiver reservado 
    
        h[i_mud-1][j_mud-1].status = '.'; //troca a letra 'R' por '.' - deixa o quarto livre    
        printf("\nReserva do apartamento (%d) do andar (%d) foi cancelada!\n", j_mud , i_mud);
        return 1;
    }
    else{
    	printf("\n\nO quarto nao esta reservado!\n");
    	return 0;
	}
}

int fCheckOut(struct stHotel h[LIN][COL]){ //realiza o check-out
	int i_mud, j_mud; //andar e quarto digitados pelo usuário
	do{ //pede testa andar/apto até que sejam válidos
        printf("Para sair e voltar ao menu, digite -1 nas opcoes!\n");
        printf ("Forneca o andar (entre 1 e 20): "); //(linha)
	scanf ("%d", &i_mud);
	printf("\n");
        LimpaEntrada();
	printf ("Forneca o numero do apartamento (entre 1 e 14): "); //(coluna)
	scanf ("%d", &j_mud);
        LimpaEntrada();

        if(fVoltar(i_mud, j_mud)){ //se for verdadeiro, sai
            return 0;
        }
        }
        while(!fQuartoValido(i_mud, j_mud));
	
	if (fQuartoOcupado(i_mud, j_mud, h)){ // se o quarto estiver ocupado 

        h[i_mud-1][j_mud-1].status = '.'; //troca a letra 'O' por '.' - deixa o quarto livre    
        printf("\n\nCheck-out do apartamento (%d) do andar (%d) realizado!\n", j_mud , i_mud);
        return 1;
    }
    else{
    	printf("\n\nO quarto nao esta ocupado!\n");
    	return 0;
	}
}



int fQuartoValido(int i_mud, int j_mud){ //verifica se andar/apto existem na matriz
    if ((i_mud > 0) && (i_mud <= LIN) && (j_mud > 0) && (j_mud <= COL))
        return 1;
    else {
	printf ("\n\nANDAR/APTO FORNECIDOS INVALIDOS!!\n\n");
        return 0;
    }
}

int fQuartoReservado(int i_mud, int j_mud, struct stHotel h[LIN][COL]){ //verifica se o quarto está reservado
	if(h[i_mud-1][j_mud-1].status == 'R')
		return 1;
	else
		return 0;
}

int fQuartoOcupado(int i_mud, int j_mud, struct stHotel h[LIN][COL]){ //verifica se o quarto está ocupado
	if(h[i_mud-1][j_mud-1].status == 'O')
		return 1;
	else
		return 0;
}

int fQuartoVazio(int i_mud, int j_mud, struct stHotel h[LIN][COL]){ //verifica se o quarto está vazio
	if(h[i_mud-1][j_mud-1].status == '.')
		return 1;
	else
		return 0;
}



void fMenuTaxa(struct stHotel h[LIN][COL]){ //mostra o menu específico para as taxas do hotel e testa as opções que o usuário digita
	int opc; //opção para selecionar entre as opções de taxa
	
	while (opc != 4){
	printf ("\nDigite 1 para ver a taxa de reserva");
	printf ("\nDigite 2 para ver a taxa de ocupacao");
	printf ("\nDigite 3 para ver a taxa de disponibilidade");
	printf ("\nDigite 4 para sair do menu");
	printf ("\nOpcao: ");
	scanf ("%d",&opc);
	
	switch(opc){
		case 1:fTaxaReserva(h);break; //se a opção for 1, mostra a taxa de reserva
		case 2:fTaxaOcupado(h);break; //se a opção for 2, mostra a taxa de ocupação
		case 3:fTaxaVazio(h);break; //se a opção for 3, mostra a taxa de quartos vazios
		case 4:break; //se a opção for 4, volta ao menu principal
		default:printf("\nOpcao invalida, digite novamente\n"); //se a opção não for nenhuma das acima, pede para digitar novamente
		}
	}
}

void fTaxaReserva(struct stHotel h[LIN][COL]){ //calcula e mostra a taxa de reserva
	int c, l, s; //c= coluna (apartamento); l= linha (andar); soma dos quartos reservados
	float y; //taxa de reservas
	s = 0;	
	for (l=1; l<=LIN; l++){ //varre o hotel e conta os quartos reservados
		for (c=1; c<=COL; c++){
			if (fQuartoReservado(l, c, h))
				s = s + 1;}}
	y = ((100. * s)/(LIN*COL)); //faz a conta
	printf("\n\nA taxa de reserva e' = %.2f %%\n\n", y);
}

void fTaxaOcupado(struct stHotel h[LIN][COL]){ //calcula e mostra a taxa de ocupação
	int c, l, s; //c= coluna (apartamento); l= linha (andar); soma dos quartos ocupados
	float y; //taxa de ocupação
	s = 0;	
	for (l=1; l<=LIN; l++){ //varre o hotel e conta os quartos ocupados
		for (c=1; c<=COL; c++){
			if (fQuartoOcupado(l, c, h))
				s = s + 1;}}
	y = ((100. * s)/(LIN*COL)); //faz a conta
	printf("\n\nA taxa de ocupacao e' = %.2f %%\n\n", y);
}

void fTaxaVazio(struct stHotel h[LIN][COL]){ //calcula e mostra a taxa de quartos vazios
	int c, l, s; //c= coluna (apartamento); l= linha (andar); soma dos quartos vazios
	float y; //taxa de quartos vazios
	s = 0;	
	for (l=1; l<=LIN; l++){ //varre o hotel e conta os quartos vazios
		for (c=1; c<=COL; c++){
			if (fQuartoVazio(l, c, h))
				s = s + 1;}}
	y = ((100. * s)/(LIN*COL)); //faz a conta
	printf("\n\nA taxa de disponibilidade e' = %.2f %%\n\n", y);
}



void fVerificarStatus(struct stHotel h[LIN][COL]){ //verifica o status de um determinado quarto
	int i_mud, j_mud; //andar e quarto digitados pelo usuário
	printf ("\nForneca o andar: ");
	scanf  ("%d",&i_mud);
	printf ("\nForneca o apartamento: ");
	scanf  ("%d",&j_mud);
	
	if (fQuartoVazio(i_mud,j_mud,h)){ //se estiver vazio, mostra uma mensagem
		printf ("\n\nO quarto %d %d esta vazio\n\n",i_mud,j_mud);
	}
	
	else if (fQuartoReservado(i_mud,j_mud,h)){ //se estiver reservado, mostra uma mensagem
		printf ("\n\nO quarto %d %d esta reservado\n\n",i_mud,j_mud);
	}
	
	else if (fQuartoOcupado(i_mud,j_mud,h)){ //se estiver ocupado, mostra os dados do cadastro do cliente
		fMostraCadastro(i_mud,j_mud,h);
	}
}

void fMostraMenu(void){ //mostra as opções que o gerente pode escolher no menu principal
    printf("\nDigite 1 para ver o mapa do hotel");
    printf("\nDigite 2 para fazer cadastro (reservar um quarto ou fazer o check-in)");
    printf("\nDigite 3 para fazer o check-in de um quarto reservado");
    printf("\nDigite 4 para cancelar a reserva de um quarto");
    printf("\nDigite 5 para fazer o check-out de um quarto");
    printf("\nDigite 6 para ver as taxas do hotel");
    printf("\nDigite 7 para mostrar o status de um quarto");
    printf("\nDigite 9 para sair do programa");
    printf("\nOpcao: ");
}



main(){
    struct stHotel Hotel[LIN][COL]; //Matriz do Hotel
    int opcao = 0; //opção do menu utilizada no loop principal
    fPreencheHotel(Hotel);
    fMostraHotel(Hotel);

while(1){ //equanto a pessoa não optar por sair
    fMostraMenu();
    scanf("%d", &opcao);
    LimpaEntrada();

    switch(opcao){
        case 1: //mostra a situação atualizada do hotel
            fMostraHotel(Hotel);
            break;
        case 2: //faz o cadastro, e seleciona um quarto para o cliente (fazer reserva ou diretamente o check-in)
            fFazerCadastro(Hotel);
            break;
        case 3: //realiza o check-in de um quarto já reservado anteriormente, sem precisar passar pelo cadastro
            fCheckInComReserva(Hotel);
            break;
        case 4: //cancela a reserva de um quarto
            fCancelarReserva(Hotel);
			break;
        case 5: //fazer check-out
            fCheckOut(Hotel);
            break;
        case 6: //visualizar taxas do hotel
            fMenuTaxa(Hotel);
            break;
        case 7: //verifica o status de um determinado quarto
        	fVerificarStatus(Hotel);
            break;
		case 9: //sai do programa
    		printf("\n\nFIM DE PROGRAMA!\n\n");
    		system("pause");
    		return 0;
    	}
    
	}

} 
