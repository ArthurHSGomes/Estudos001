# Estudos001 

public enum StatusEntrega {
    PENDENTE,
    ATRIBUIDA,
    FINALIZADA
}

//rota.java
public class Rota {
    private String origem;
    private String destino;
    private double distanciaTotal;
    private double tempoEstimado;

    public Rota(String origem, String destino, double distanciaTotal, double tempoEstimado) {
        this.origem = origem;
        this.destino = destino;
        this.distanciaTotal = distanciaTotal;
        this.tempoEstimado = tempoEstimado;
    }

    public String getOrigem() { return origem; }
    public String getDestino() { return destino; }

    @Override
    public String toString() {
        return origem + " -> " + destino + " (" + distanciaTotal + "km, " + tempoEstimado + "h)";
    }
}

//motorista.java

public class Motorista {
    private String nome;
    private String cpf;
    private String categoriaCNH;
    private String telefone;

    public Motorista(String nome, String cpf, String categoriaCNH, String telefone) {
        this.nome = nome;
        this.cpf = cpf;
        this.categoriaCNH = categoriaCNH;
        this.telefone = telefone;
    }

    public String getNome() { return nome; }

    @Override
    public String toString() {
        return nome + " (CNH: " + categoriaCNH + ")";
    }
}


//entrega.java

public class Entrega {
    private String codigo;
    private String destinatario;
    private String endereco;
    private String data;
    private double peso;
    private StatusEntrega status;
    private Rota rota;
    private Veiculo veiculo;

    public Entrega(String codigo, String destinatario, String endereco,
                   String data, double peso, Rota rota) {
        this.codigo = codigo;
        this.destinatario = destinatario;
        this.endereco = endereco;
        this.data = data;
        this.peso = peso;
        this.rota = rota;
        this.status = StatusEntrega.PENDENTE;
    }

    public String getCodigo() { return codigo; }
    public StatusEntrega getStatus() { return status; }
    public Veiculo getVeiculo() { return veiculo; }

    public void setStatus(StatusEntrega status) { this.status = status; }
    public void setVeiculo(Veiculo veiculo) { this.veiculo = veiculo; }

    @Override
    public String toString() {
        return "Entrega " + codigo +
               " | " + destinatario +
               " | Status: " + status +
               " | Veículo: " + (veiculo != null ? veiculo.getPlaca() : "nenhum");
    }
}

//veiculo.java

public void listarEntregas() {
    for (int i = 0; i < entregas.size(); i++) {
        System.out.println(entregas.get(i));
    }
}

//sistemalogistica.java

import java.util.ArrayList;
import java.util.List;

public class SistemaLogistica {
    private List<Veiculo> veiculos = new ArrayList<>();
    private List<Entrega> entregas = new ArrayList<>();

    public void addVeiculo(Veiculo v) { veiculos.add(v); }
    public void addEntrega(Entrega e) { entregas.add(e); }

    public void buscarPorStatus(StatusEntrega status) {
        System.out.println("\n=== Entregas com status: " + status + " ===");

        boolean encontrou = false;

        for (int i = 0; i < entregas.size(); i++) {
            Entrega e = entregas.get(i);

            if (e.getStatus() == status) {
                System.out.println(e);
                encontrou = true;
            }
        }

        if (!encontrou) {
            System.out.println("Nenhuma entrega encontrada.");
        }
    }

    public void relatorio() {
        for (int i = 0; i < veiculos.size(); i++) {
            Veiculo v = veiculos.get(i);

            System.out.println(v.getPlaca() + ": " +
                v.contarFinalizadas(entregas) + " finalizadas");
        }
    }
}

//main.java

public class Main {
    public static void main(String[] args) {

        SistemaLogistica sistema = new SistemaLogistica();

        Motorista m1 = new Motorista("Carlos", "111", "C", "999");
        Veiculo v1 = new Veiculo("ABC-1234", "Van");

        v1.associarMotorista(m1);
        sistema.addVeiculo(v1);

        Rota r = new Rota("BH", "SP", 500, 6);

        Entrega e1 = new Entrega("E001", "João", "SP", "2026", 100, r);
        sistema.addEntrega(e1);

        v1.atribuirEntrega(e1);
        v1.listarEntregas();

        v1.finalizarEntrega(e1);

        sistema.relatorio();
        sistema.buscarPorStatus(StatusEntrega.FINALIZADA);
    }
}







