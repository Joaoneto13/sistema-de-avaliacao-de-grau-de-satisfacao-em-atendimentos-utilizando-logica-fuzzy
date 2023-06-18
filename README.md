# sistema-de-avaliacao-de-grau-de-satisfacao-em-atendimentos-utilizando-logica-fuzzy
Trabalho realizado em python com base na lógica fuzzy, na avaliação da disciplina de programação avançada de computadores, do curso de sistemas de informação da Universidade Federal do Pará
import numpy as np
import skfuzzy as fuzz
from skfuzzy import control as ctrl

# Antecedentes
tratamento = ctrl.Antecedent(np.arange(0, 11, 1), 'tratamento')
tempo_espera = ctrl.Antecedent(np.arange(0, 11, 1), 'tempo_espera')
efetividade = ctrl.Antecedent(np.arange(0, 11, 1), 'efetividade')

# Consequente
satisfacao = ctrl.Consequent(np.arange(0, 11, 1), 'satisfacao')

# Funções de pertinência
tratamento['ruim'] = fuzz.trimf(tratamento.universe, [0, 0, 5])
tratamento['regular'] = fuzz.trimf(tratamento.universe, [5, 6, 7])
tratamento['bom'] = fuzz.trimf(tratamento.universe, [7, 10, 10])

tempo_espera['ruim'] = fuzz.trimf(tempo_espera.universe, [0, 0, 5])
tempo_espera['regular'] = fuzz.trimf(tempo_espera.universe, [5, 6, 7])
tempo_espera['bom'] = fuzz.trimf(tempo_espera.universe, [7, 10, 10])

efetividade['ruim'] = fuzz.trimf(efetividade.universe, [0, 0, 5])
efetividade['regular'] = fuzz.trimf(efetividade.universe, [5, 6, 7])
efetividade['bom'] = fuzz.trimf(efetividade.universe,[7 ,10 ,10])

satisfacao['ruim'] = fuzz.trimf(satisfacao.universe,[0 ,0 ,5])
satisfacao['regular'] = fuzz.trimf(satisfacao.universe,[5 ,6 ,7])
satisfacao['bom'] = fuzz.trimf(satisfacao.universe,[7 ,10 ,10])

# Regras
rule1 = ctrl.Rule(tratamento['ruim'] | tempo_espera['ruim'] | efetividade['ruim'], satisfacao['ruim'])
rule2 = ctrl.Rule(tratamento['regular'] & tempo_espera['regular'] & efetividade['regular'], satisfacao['regular'])
rule3 = ctrl.Rule(tratamento['bom'] & tempo_espera['bom'] & efetividade['bom'], satisfacao['bom'])

# Sistema de controle
satisfacao_ctrl = ctrl.ControlSystem([rule1, rule2, rule3])
satisfacao_simulador = ctrl.ControlSystemSimulation(satisfacao_ctrl)

# Entrada de dados
satisfacao_simulador.input['tratamento'] = float(input("Tratamento do atendente (0-10): "))
satisfacao_simulador.input['tempo_espera'] = float(input("Tempo de espera (0-10): "))
satisfacao_simulador.input['efetividade'] = float(input("Efetividade das soluções (0-10): "))

# Cálculo da satisfação do cliente
satisfacao_simulador.compute()
print(f"Satisfação do cliente: {satisfacao_simulador.output['satisfacao']}")

# Gráfico da satisfação do cliente
satisfacao.view(sim=satisfacao_simulador)
