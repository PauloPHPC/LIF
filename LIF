import pathlib
import pypdfium2 as pdfium
import re
import csv

def PastaArquivos(pathInicial):
    listFile = []
    for filepath in pathlib.Path(pathInicial).glob("**/*.pdf"):
        listFile.append(filepath.absolute())
    return listFile

def ExtraiTexto(pathPDF):
    pdf = pdfium.PdfDocument(pathPDF)
    
    if len(pdf) == 2:
        
        page1 = pdf[0]
        page2 = pdf[1]
        page1 = page1.get_textpage()
        page2 = page2.get_textpage()

        page1 = page1.get_text_range()
        page2 = page2.get_text_range()


        projeto = re.findall("PROJETO: .*", page1)
        projeto = str(projeto[0])
        projeto = projeto[9:-1]

        incentivador = re.findall("CONTRIBUINTE INCENTIVADOR: .*", page1)
        incentivador = str(incentivador[0])
        incentivador = incentivador[27:-1]

        cpf_cnpj = re.findall("CPF/CNPJ: .*", page1)
        cpf_cnpj = str(cpf_cnpj[0])
        cpf_cnpj = cpf_cnpj[10:-1]

        valorImposto = re.findall("VALOR DO IMPOSTO: .*", page1)
        valorImposto = str(valorImposto)
        valorImposto = valorImposto[23:-4]
        

        if re.findall("ISSQN", page1) == ['ISSQN']:
            tpoImposto = "ISSQN"
        else:
            tpoImposto = "IPTU"

        terceiro = re.findall("TERCEIRO: .*", page2) or re.findall("TECEIRO: .*", page2)
        terceiro = str(terceiro[0])
        terceiro = terceiro[10:-1]

        cpf_cnpj_terceiro = re.findall("CPF/CNPJ: .*", page2)
        cpf_cnpj_terceiro = str(cpf_cnpj_terceiro[0])
        cpf_cnpj_terceiro = cpf_cnpj_terceiro[10:-1]

        valorContra = re.findall("Valor Total: .*", page2)
        valorContra = str(valorContra[0])
        valorContra = valorContra[16:-1]
    

        lista_retorno = [projeto, incentivador, cpf_cnpj, tpoImposto, valorImposto, terceiro, cpf_cnpj_terceiro, valorContra]

        return lista_retorno

    else:

        page1 = pdf[0]
        page1 = page1.get_textpage()

        page1 = page1.get_text_range()

        
        projeto = re.findall("PROJETO: .*", page1)
        projeto = str(projeto[0])
        projeto = projeto[9:-1]

        incentivador = re.findall("CONTRIBUINTE INCENTIVADOR: .*", page1)
        incentivador = str(incentivador[0])
        incentivador = incentivador[27:-1]

        cpf_cnpj = re.findall("CPF/CNPJ: .*", page1)
        cpf_cnpj = str(cpf_cnpj[0])
        cpf_cnpj = cpf_cnpj[10:-1]

        valorImposto = re.findall("VALOR DO IMPOSTO: .*", page1)
        valorImposto = str(valorImposto)
        valorImposto = valorImposto[23:-4]
        
        if re.findall("ISSQN", page1) == ['ISSQN']:
            tpoImposto = "ISSQN"
        else:
            tpoImposto = "IPTU"

        valorContra = re.findall("CONTRAPARTIDA: .*", page1)
        valorContra = str(valorContra[0])
        valorContra = valorContra[18:-1]

        lista_retorno = [projeto, incentivador, cpf_cnpj, tpoImposto, valorImposto, 0, 0, valorContra]
    
        return lista_retorno

def GerarCSV(lista):
    header = ["Projeto", "Incentivador", "CPF/CNPJ", "Tipo Imposto", "Valor", "Terceiro", "CPF/CNPJ Terceiro"]
    
    with open(r"D:\LIF\ERROS\LIF.csv",'w', newline = "") as csvfile:
        writer = csv.writer(csvfile)
        writer.writerow(header)
        #writer.writerows(lista)
        for i in range(len(lista)):       
            writer.writerow(lista[i])
    
    return print("Sucesso!!")

path = r"D:\LIF\ERROS"

listaArquivos = PastaArquivos(path)
print(listaArquivos)
lista2 = []

for i in range(len(listaArquivos)):
    lista1 = []
    lista1 = ExtraiTexto(str(listaArquivos[i]))
    lista2.append(lista1)

arquivoFinal = GerarCSV(lista2)
    