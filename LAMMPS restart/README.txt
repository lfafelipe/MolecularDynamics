### CONFIGURAÇÕES NECESSÁRIAS PARA REINICIALIZAÇÃO DE UMA SIMULAÇÃO LAMMPS

# Intro: 

Pode ser de interesse do usuário executar várias simulações de um mesmo experimento em que apenas uma etapa da simulção será 
diferente em cada caso. Ou seja, a simulação pode ser interpretada como uma sequencia de blocos de execução onde os demais 
testes serão diferentes a partir de, ou apenas em, alguns blocos (fixes). 

# Procedimento:

A simulação pode ser salva em um arquivo de reinicialização em qualquer momento do experimento, ficando a critério do usuário
definir o momento (timestep) em que o arquivo restart será gerado. O arquivo de reinicialização pode ser gerado pelo comando
write_restart (dica: utilizar * no nome do arquivo para indicar o timestep em que ocorreu a gravação). 

São necessários dois arquivos no diretório para que a simulação seja reinicializada:

1 - Input file com o comando read_restart;
2 - O arquivo de reinicialização gerado pelo write_restart.

# Orientações Restart file:

O nome do arquivo deverá seguir a codificação abaixo:

TIPO.FF.SISTEMA.METODO

- Tipo: in (input); restart (reinicialização);
- FF: campo de força;
- SISTEMA: abreviação/ID da(s) molécula(s);
- METODO: objetivo da simulação (ex.: GKvisc - viscosidade Green-Kubo, NEMDvisc - viscosidade NEMD);

Ex.: 
restart.LOPLS.nc16.NEMDvisc.ver0

As informações abaixo devem obrigatoriamente fazer parte do restart input file:

- fix: usar o mesmo ID do input file original;
- compute: usar o mesmo ID do input file original;
- variables: declarar novamente todas as variáveis;
- region: definir novamente a região de referência da caixa;
- neighbor_list: repetir o comando do input file (ou modificar caso seja de interesse);
- kspace_style: repetir o comando do input file (ou modificar caso seja de interesse);
- dump/restart files: declarar o comando novamente, caso seja de interesse;

* ATENÇÃO: O manual do LAMMPS indica que não é necessário repetir a declaração dos coeficientes de interação molecular.
Contudo, situações em que uma interação seja calculada pelo style hybrid (ex.: L-OPLS) necessitam que os coeficientes sejam
declarados novamente. 

Os comandos abaixo não precisam ser declarados novamente: 

- units;
- atom_style & atom_modify;
- timestep;
- boundary;
- Simulation box (data file);
- Atoms settings (mass) & attributes (bonds, angles, dihedral, etc.);
- Force field style & coeffs;
- Pair modify.


 