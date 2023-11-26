import random

class AgenteEconomico:
    def __init__(self, estrategia, saldo_inicial=200):
        self.estrategia = estrategia
        self.saldo = saldo_inicial
        self.historial_saldos = []

    def tomar_decision(self):
        return self.estrategia

    def actualizar_saldo(self, recompensa):
        self.saldo += recompensa
        self.historial_saldos.append(self.saldo)

    def cambiar_estrategia(self, tasa_mutacion=0.1):
        if random.random() < tasa_mutacion:
            self.estrategia = not self.estrategia

def jugar_juego(agentes):
    for i in range(len(agentes)):
        for j in range(i + 1, len(agentes)):
            agente1 = agentes[i]
            agente2 = agentes[j]
            decision1 = agente1.tomar_decision()
            decision2 = agente2.tomar_decision()

            recompensa1, recompensa2 = calcular_recompensas(decision1, decision2)
            agente1.actualizar_saldo(recompensa1)
            agente2.actualizar_saldo(recompensa2)

def calcular_recompensas(decision1, decision2):
    if decision1 and decision2:
        return (4, 4)
    elif decision1 and not decision2:
        return (0, 6)
    elif not decision1 and decision2:
        return (6, 0)
    else:
        return (2, 2)

def mostrar_saldos(agentes):
    for i, agente in enumerate(agentes):
        print(f"Agente {i + 1}: Saldo = ${agente.saldo}")

def simulacion(numero_agentes, numero_iteraciones):
    agentes = [AgenteEconomico(random.choice([True, False])) for _ in range(numero_agentes)]

    for _ in range(numero_iteraciones):
        jugar_juego(agentes)

        # Cambiar estrategias de agentes basado en su desempeño pasado
        for agente in agentes:
            agente.cambiar_estrategia()

    mostrar_saldos(agentes)

    # Devolver historial de saldos de los agentes
    return [agente.historial_saldos for agente in agentes]

if __name__ == "__main__":
    numero_agentes = 8
    numero_iteraciones = 15
    historiales_saldos = simulacion(numero_agentes, numero_iteraciones)

    # Imprimir historial de saldos de cada agente
    for i, historial in enumerate(historiales_saldos):
        print(f"\nHistorial de saldos para Agente {i + 1}: {historial}")
