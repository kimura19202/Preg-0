# Preg-0
Artigo para Avaliação Final do Trabalho de Conclusão de Curso


# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session



### R BLOCK ###

library(readr)
WineQT <- read_csv("WineQT.csv")
ref <- subset(WineQT, select=c("quality", "Id"))
ref$Id <- c(1:1143)
ref <- ref[, c(2,1)]

WineQT$quality <- NULL
WineQT$Id <- NULL
vinhos <- scale.default(WineQT)
vinhos <- transform(vinhos)

write.csv(vinhos,"C:\\Users\\rafae\\OneDrive\\Documentos\\Database\\vinhos.csv", row.names = FALSE)
write.csv(ref,"C:\\Users\\rafae\\OneDrive\\Documentos\\Database\\ref.csv", row.names = FALSE)

cereal <- read_csv("cerealsO.csv")
ref2 <- subset(cereal, select=c("name", "mfr", "type", "rating"))
ref2$Id <-(1:77)

equil <- data.frame(calories = cereal$calories / cereal$cups,
  protein = cereal$protein / cereal$cups,
  fat = cereal$fat / cereal$cups,
  sodium = cereal$sodium / cereal$cups,
  fiber = cereal$fiber / cereal$cups,
  carbo = cereal$carbo / cereal$cups,
  sugars = cereal$sugars / cereal$cups,
  potass = cereal$potass / cereal$cups,
  vitamins = cereal$vitamins / cereal$cups,
  weight = cereal$weight / cereal$cups,
  cups = cereal$cups / cereal$cups)

graus2 <- scale.default(equil)
graus2 <- transform(graus2)
graus2$cups <- NULL

write.csv(graus2,"C:\\Users\\rafae\\OneDrive\\Documentos\\Database\\cereal.csv", row.names = FALSE)
write.csv(ref2,"C:\\Users\\rafae\\OneDrive\\Documentos\\Database\\ref2.csv", row.names = FALSE)

ref3 <- read_csv("milknew.csv")
p <- round(runif(1059, 5, 15),2)
energia <- c(round(runif(1059,400,500),0))
carbo <- c(round(runif(1059,6,10),1))
prot <- c(round(runif(1059,6,8),1))
gordr <- c(round(runif(1059,5,8),1))
sodio <- c(round(runif(1059,90,150),1))
calcio <- c(round(runif(1059,200,350),1))
leite <- data.frame(energia, carbo, prot, gordr, sodio, calcio)

ref3$preco <- cbind(p)
ref3$Id <- (1:1059)
leite <- scale.default(leite)
leite <- transform(leite)

write.csv(leite,"C:\\Users\\rafae\\OneDrive\\Documentos\\Database\\leite.csv", row.names = FALSE)
write.csv(ref3,"C:\\Users\\rafae\\OneDrive\\Documentos\\Database\\ref3.csv", row.names = FALSE)

### END ###



vinhos = pd.read_csv("/kaggle/input/teste1/vinhos.csv")
ref1 = pd.read_csv("/kaggle/input/teste1/ref.csv")

graus = pd.DataFrame(data = vinhos.sum(axis = 1))
graus = graus.rename(columns = {0 : "otimização"})
graus = graus.abs()

df1 = {"ID": ref1["Id"],
       "Otimização": graus["otimização"],
       "Qualidade": ref1["quality"]}
df1 = pd.DataFrame(data = df1)

teste1 = df1.sort_values(by = "Otimização")
teste1.head(10)



cereal = pd.read_csv("/kaggle/input/teste2/cereal.csv")
ref2 = pd.read_csv("/kaggle/input/teste2/ref2.csv")

graus2 = pd.DataFrame(data = cereal.sum(axis = 1))
graus2 = graus2.rename(columns = {0 : "otimização"})
graus2 = graus2.abs()

df2 = {"ID": ref2["Id"],
       "Otimização": graus2["otimização"], 
       "Pontuação": ref2["rating"],
       "Fabricante": ref2["mfr"],
       "Tipo": ref2["type"]}
df2 = pd.DataFrame(data = df2)

teste2 = df2[df2["Pontuação"] >= 50]
teste2 = teste2[teste2["Tipo"].str.contains("C")]
teste2 = teste2.sort_values(by = "Otimização")
teste2.head(10)



leite = pd.read_csv("/kaggle/input/teste-3/leite.csv")
ref3 = pd.read_csv("/kaggle/input/teste-3/ref3.csv")

graus3 = pd.DataFrame(data = leite.sum(axis = 1))
graus3 = graus3.rename(columns = {0 : "otimização"})
graus3 = graus3.abs()

df3 = {"ID": ref3["Id"], 
       "Otimização": graus3["otimização"],
       "pH": ref3["pH"],
       "Temperatura": ref3["Temprature"],
       "Sabor": ref3["Taste"],
       "Cheiro": ref3["Odor"],
       "Gordura": ref3["Fat"],
       "Turbidez": ref3["Turbidity"],
       "Cor": ref3["Colour"],
       "Avaliação": ref3["Grade"],
       "Preço": ref3["preco"]}
df3 = pd.DataFrame(data = df3)

filtro = df3[df3["Avaliação"].str.contains("h")]
filtro = filtro[filtro["Otimização"] < 0.5]
filtro = filtro[filtro["pH"] < 6.90]
filtro = filtro[filtro["pH"] > 6.25]
filtro = filtro[filtro["Temperatura"] < 45.2]
filtro = filtro[filtro["Temperatura"] > 34.0]
filtro = filtro[filtro["Sabor"] == 1]
filtro = filtro[filtro["Cheiro"] == 1]
filtro = filtro[filtro["Preço"] >= 5.49]

teste2 = filtro.sort_values(by = ["Preço","Otimização"], ascending = [True, True])
teste2.head(10)
