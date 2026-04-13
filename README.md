model/
- Organizador.java
- Salao.java
- Reserva.java
- StatusReserva.java
- Periodo.java

service/
- SistemaEventos.java

main/
- Main.java

//statusreserva.java

public enum StatusReserva {
    SOLICITADA,
    CONFIRMADA,
    FINALIZADA
}

//periodo.java

public enum Periodo {
    MANHA,
    TARDE,
    NOITE
}

//organizador.java
public class Organizador {
    private String nome;
    private String cpf;
    private String telefone;
    private String areaAtuacao;
    private Salao salao; // 1 organizador → 1 salão

    public Organizador(String nome, String cpf, String telefone, String areaAtuacao) {
        this.nome = nome;
        this.cpf = cpf;
        this.telefone = telefone;
        this.areaAtuacao = areaAtuacao;
    }

    public String getNome() { return nome; }

    public Salao getSalao() { return salao; }

    public void setSalao(Salao salao) {
        this.salao = salao;
    }

    @Override
    public String toString() {
        return nome + " (" + areaAtuacao + ")";
    }
}
//salao.java

import java.util.ArrayList;
import java.util.List;

public class Salao {
    private int numero;
    private int capacidade;
    private String localizacao;
    private String tipo;
    private Organizador organizador;
    private List<Reserva> reservas = new ArrayList<>();

    public Salao(int numero, int capacidade, String localizacao, String tipo) {
        this.numero = numero;
        this.capacidade = capacidade;
        this.localizacao = localizacao;
        this.tipo = tipo;
    }

    public int getNumero() { return numero; }
    public Organizador getOrganizador() { return organizador; }
    public List<Reserva> getReservas() { return reservas; }

    // Funcionalidade 1
    public void associarOrganizador(Organizador o) {
        if (o.getSalao() != null) {
            System.out.println("Organizador já possui salão.");
            return;
        }
        this.organizador = o;
        o.setSalao(this);
    }

    // Funcionalidade 2
    public void adicionarReserva(Reserva r) {

        if (r.getStatus() != StatusReserva.SOLICITADA) {
            System.out.println("Reserva inválida.");
            return;
        }

        // verificar conflito de data + período
        for (int i = 0; i < reservas.size(); i++) {
            Reserva existente = reservas.get(i);

            if (existente.getData().equals(r.getData()) &&
                existente.getPeriodo() == r.getPeriodo()) {

                System.out.println("Conflito de horário!");
                return;
            }
        }

        r.setSalao(this);
        r.setStatus(StatusReserva.CONFIRMADA);
        reservas.add(r);
    }

    // Funcionalidade 3
    public void exibirReservasConfirmadas() {
        int count = 0;

        for (int i = 0; i < reservas.size(); i++) {
            Reserva r = reservas.get(i);

            if (r.getStatus() == StatusReserva.CONFIRMADA) {
                System.out.println(r);
                count++;
            }
        }

        System.out.println("Total: " + count);
    }

    // Funcionalidade 4
    public int contarFinalizadas(List<Reserva> todas) {
        int count = 0;

        for (int i = 0; i < todas.size(); i++) {
            Reserva r = todas.get(i);

            if (r.getStatus() == StatusReserva.FINALIZADA &&
                r.getSalao() == this) {
                count++;
            }
        }

        return count;
    }
}

//reserva.java

public class Reserva {
    private String codigo;
    private String cliente;
    private String data;
    private Periodo periodo;
    private int convidados;
    private StatusReserva status;
    private double valor;
    private Salao salao;

    public Reserva(String codigo, String cliente, String data,
                   Periodo periodo, int convidados, double valor) {
        this.codigo = codigo;
        this.cliente = cliente;
        this.data = data;
        this.periodo = periodo;
        this.convidados = convidados;
        this.valor = valor;
        this.status = StatusReserva.SOLICITADA;
    }

    public String getCodigo() { return codigo; }
    public String getData() { return data; }
    public Periodo getPeriodo() { return periodo; }
    public StatusReserva getStatus() { return status; }
    public Salao getSalao() { return salao; }

    public void setStatus(StatusReserva status) { this.status = status; }
    public void setSalao(Salao salao) { this.salao = salao; }

    @Override
    public String toString() {
        return "Reserva " + codigo +
               " | Cliente: " + cliente +
               " | Status: " + status;
    }
}

//sistemaeventos.java

import java.util.ArrayList;
import java.util.List;

public class SistemaEventos {

    private List<Salao> saloes = new ArrayList<>();
    private List<Reserva> reservas = new ArrayList<>();

    public void addSalao(Salao s) { saloes.add(s); }
    public void addReserva(Reserva r) { reservas.add(r); }

    // Funcionalidade 4
    public void relatorioFinalizadas() {
        for (int i = 0; i < saloes.size(); i++) {
            Salao s = saloes.get(i);

            System.out.println("Salão " + s.getNumero() +
                ": " + s.contarFinalizadas(reservas));
        }
    }

    // Funcionalidade 5
    public void buscarPorStatus(StatusReserva status) {

        for (int i = 0; i < reservas.size(); i++) {
            Reserva r = reservas.get(i);

            if (r.getStatus() == status) {
                System.out.println(r);

                if (r.getSalao() != null) {
                    System.out.println("Salão: " + r.getSalao().getNumero());

                    if (r.getSalao().getOrganizador() != null) {
                        System.out.println("Organizador: " +
                            r.getSalao().getOrganizador());
                    }
                }
            }
        }
    }

    // Funcionalidade 6
    public void detalhesReserva(String codigo) {
        for (int i = 0; i < reservas.size(); i++) {
            Reserva r = reservas.get(i);

            if (r.getCodigo().equals(codigo)) {
                System.out.println(r);
                return;
            }
        }
    }
}

//main.java

public class Main {

    public static void main(String[] args) {

        SistemaEventos sistema = new SistemaEventos();

        // 🔥 Criar 3 organizadores
        Organizador o1 = new Organizador("Carlos", "111", "999", "Casamentos");
        Organizador o2 = new Organizador("Ana", "222", "888", "Festas");
        Organizador o3 = new Organizador("Pedro", "333", "777", "Eventos");

        // 🔥 Criar 3 salões
        Salao s1 = new Salao(1, 200, "Centro", "Luxo");
        Salao s2 = new Salao(2, 100, "Bairro", "Simples");
        Salao s3 = new Salao(3, 300, "Zona Sul", "Premium");

        sistema.addSalao(s1);
        sistema.addSalao(s2);
        sistema.addSalao(s3);

        // Funcionalidade 1
        s1.associarOrganizador(o1);
        s2.associarOrganizador(o2);
        s3.associarOrganizador(o3);

        // Criar reservas
        Reserva r1 = new Reserva("R1", "João", "10/05", Periodo.MANHA, 50, 1000);
        Reserva r2 = new Reserva("R2", "Maria", "10/05", Periodo.TARDE, 80, 2000);
        Reserva r3 = new Reserva("R3", "Lucas", "11/05", Periodo.NOITE, 120, 3000);

        sistema.addReserva(r1);
        sistema.addReserva(r2);
        sistema.addReserva(r3);

        // Funcionalidade 2
        s1.adicionarReserva(r1);
        s1.adicionarReserva(r2);
        s2.adicionarReserva(r3);

        // Funcionalidade 3
        s1.exibirReservasConfirmadas();

        // Finalizar
        r1.setStatus(StatusReserva.FINALIZADA);

        // Funcionalidade 4
        sistema.relatorioFinalizadas();

        // Funcionalidade 5
        sistema.buscarPorStatus(StatusReserva.CONFIRMADA);

        // Funcionalidade 6
        sistema.detalhesReserva("R1");
    }
}



EstudosEventos/
│
├── src/
│   ├── model/
│   │   ├── Organizador.java
│   │   ├── Salao.java
│   │   ├── Reserva.java
│   │   ├── StatusReserva.java
│   │   └── Periodo.java
│   │
│   ├── service/
│   │   └── SistemaEventos.java
│   │
│   └── main/
│       └── Main.java
│
├── lib/            (opcional)
├── bin/ ou out/    (gerado automaticamente)
└── .vscode/



package main;

import model.*;
import service.*;

public class Main {

    public static void main(String[] args) {

        SistemaEventos sistema = new SistemaEventos();

        // 🔥 Criar 3 organizadores
        Organizador o1 = new Organizador("Carlos", "111", "999", "Casamentos");
        Organizador o2 = new Organizador("Ana", "222", "888", "Festas");
        Organizador o3 = new Organizador("Pedro", "333", "777", "Eventos");

        // 🔥 Criar 3 salões
        Salao s1 = new Salao(1, 200, "Centro", "Luxo");
        Salao s2 = new Salao(2, 100, "Bairro", "Simples");
        Salao s3 = new Salao(3, 300, "Zona Sul", "Premium");

        sistema.addSalao(s1);
        sistema.addSalao(s2);
        sistema.addSalao(s3);

        // 🔥 Funcionalidade 1: Associar organizadores
        s1.associarOrganizador(o1);
        s2.associarOrganizador(o2);
        s3.associarOrganizador(o3);

        // 🔥 Criar reservas
        Reserva r1 = new Reserva("R1", "João", "10/05", Periodo.MANHA, 50, 1000);
        Reserva r2 = new Reserva("R2", "Maria", "10/05", Periodo.TARDE, 80, 2000);
        Reserva r3 = new Reserva("R3", "Lucas", "11/05", Periodo.NOITE, 120, 3000);

        sistema.addReserva(r1);
        sistema.addReserva(r2);
        sistema.addReserva(r3);

        // 🔥 Funcionalidade 2: Atribuir reservas
        s1.adicionarReserva(r1);
        s1.adicionarReserva(r2);
        s2.adicionarReserva(r3);

        // 🔥 Funcionalidade 3
        System.out.println("\n=== Reservas confirmadas no Salão 1 ===");
        s1.exibirReservasConfirmadas();

        // 🔥 Finalizar algumas reservas
        r1.setStatus(StatusReserva.FINALIZADA);

        // 🔥 Funcionalidade 4
        System.out.println("\n=== Relatório de finalizadas ===");
        sistema.relatorioFinalizadas();

        // 🔥 Funcionalidade 5
        System.out.println("\n=== Buscar por status CONFIRMADA ===");
        sistema.buscarPorStatus(StatusReserva.CONFIRMADA);

        // 🔥 Funcionalidade 6
        System.out.println("\n=== Detalhes da reserva R1 ===");
        sistema.detalhesReserva("R1");
    }
}
