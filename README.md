# orm  
##projeto utilizando orm no python 


**Em primeiro lugar executar o comando para instalar o django**  
pip install django

**Após isso criar um projeto**  
django-admin startproject orm .

**criar uma aplicação**    
django-admin startapp core


**Criar tabela no banco de dados**   
python manage.py makemigrations
python manage.py migrate

**Criar super usuário**  
python manage.py createsuperuser


*********************************************************************************


**Criação dos modelos e relacionamentos.**

from django.db import models
#importar o getusermodel do admin
from django.contrib.auth import get_user_model


#Estrutura base de um veiculo, númedo de chassi  
class Chassi(models.Model):  
    numero = models.CharField('Chassi', max_length=16, help_text='Máximo 16 caracteres')  

    class Meta:  
        verbose_name = 'Chassi'  
        verbose_name_plural = 'Chassis'  

    def __str__(self):  
        return self.numero  
        
        
        


class Montadora(models.Model):  
    nome = models.CharField('Nome', max_length=50)  

    class Meta:  
        verbose_name = 'Montadora'  
        verbose_name_plural = 'Montadoras'  

    def __str__(self):  
        return self.nome  

#get_or_create - pega uma montadora chamada padrão, ou caso não exista cria essa montadora  
def set_default_montadora():  
    return Montadora.objects.get_or_create(nome='Padrão')[0]  # (objeto, boolean)  


class Carro(models.Model):  
    """  
    # OneToOneField  
    Cada carro só pode se relacionar com um chassi  
    e cada chassi só pode se relacionar com um carro.  

    # ForeignKey (One to Many)  
    Cada carro tem uma montadora mas uma montadora  
    pode 'montar' vários carros.  

    # ManyToMany  
    Um carro pode ser dirigido por vários motoristas  
    e um motorista pode dirigir diversos carros.  
    """  
    chassi = models.OneToOneField(Chassi, on_delete=models.CASCADE)  
    #uma montadora pode ter vários carros  
    montadora = models.ForeignKey(Montadora, on_delete=models.SET(set_default_montadora))  
    motoristas = models.ManyToManyField(get_user_model())  
    modelo = models.CharField('Modelo', max_length=30, help_text='Máximo 30 caracteres')  
    preco = models.DecimalField('Preço', max_digits=8, decimal_places=2)  

    class Meta:  
        verbose_name = 'Carro'  
        verbose_name_plural = 'Carros'  

    def __str__(self):  
        return f'{self.montadora} {self.modelo}'  
       
*********************************************************************************
        
        














