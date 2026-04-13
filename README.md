Analise o cenário abaixo e desenvolva um sistema para atender à demanda solicitada.
Seu programa deve seguir todos os conceitos da Programação Orientada a Objetos e
implementá-los de forma correta. Leia com atenção todo o enunciado antes de iniciar.
Valor: 20 pontos
Critérios de Correção:
• Aplicação correta dos conceitos de modularização e POO: 10,0 pontos.
• Implementação correta das funcionalidades solicitadas: 6,0 pontos.
• Interação com o usuário e boas práticas de programação: 4,0 pontos.
Motivos que podem zerar sua prova:
• O código não está em Java
• O código não está orientado a objetos
• O código é cópia de terceiros
• Você acessou a internet durante a prova
Cenário
Uma empresa de eventos deseja informatizar o sistema de gerenciamento de salões e reser-
vas.
Cada organizador possui as seguintes informações: nome, CPF, telefone e área de atuação.
Cada salão possui as seguintes informações: número, capacidade máxima de pessoas, loca-
lização e tipo de salão.
Cada salão está vinculado a um organizador responsável.
Para cada reserva, devem ser armazenadas informações como: código, nome do cliente,
data, horário (manhã, tarde ou noite), quantidade de convidados, status da reserva (soli-
citada, confirmada, finalizada), valor total.
Cada salão pode ter várias reservas confirmadas, contanto que estas reservas estejam em
datas diferentes, ou na mesma data mas em horário diferente.
1
Funcionalidades do Sistema
1. Associar um organizador a um salão
2. Atribuir reserva a um salão
3. Exibir todas as reservas confirmadas em um salão específico. Informe também o total
de reservas ao final.
4. Informar a quantidade total de reservas finalizadas para cada salão
5. Buscar reservas por status. É necessário exibir os detalhes da reserva, incluindo salão
e organizador.
6. Exibir os detalhes completos de uma reserva específica
Faça um método na classe main para criar 3 salões e 3 organizadores assim que o programa
for executado (sem interação com o usuário).
Regras de negócio
1. Um organizador pode ser responsável por apenas um salão.
2. Um salão pode possuir várias reservas, mas a reserva não depende do salão.
3. Reservas com status ”solicitada” não possuem salão atribuído.
4. Reservas com status ”finalizada” precisam manter a informação do salão utilizado.
Porém, o salão não precisa manter a informação desta reserva.
