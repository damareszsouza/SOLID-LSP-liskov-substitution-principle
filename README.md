# SOLID-LSP-liskov-substitution-principle


Liskov Substitution Principle (LSP)
O Princípio de Substituição de Liskov diz que objetos podem ser substituídos por seus subtipos sem que isso afete a execução correta do programa.

Exemplo


class EmailTemplate
  attr_reader :recipient

  def initialize(recipient)
    @recipient = recipient
  end

  def headers
    # code for headers
  end

  def body
    # subclasses should implement this method. It should return a string
    raise NotImplementedError
  end

  def signature
    # code for signature
  end
end

class WelcomeEmail < EmailTemplate
  def body
    # welcome body code. String content
  end
end

class InviteEmail < EmailTemplate
  def body
    {
      h1: 'You received an invitation',
      p: {
        text: 'To start your registration click on the link',
        a: '/invitation'
      }
    }
  end

  def signature
    raise NotImplementedError
  end
end



Nesse exemplo, EmailTemplate é a classe base, indicando que cada classe que herdá-la deve implementar o método body. A classe WelcomeEmail faz a implementação desse método, enquanto InviteEmail implementa body e sobrescreve signature.

Agora, supondo que uma classe receba um objeto de uma das classes de Email (EmailTemplate, WelcomeEmail, InviteEmail) e faça chamadas dos três métodos: headers, body e signature. Qualquer subclasse de EmailTemplate deveria ser utilizada sem afetar o funcionamento correto da aplicação. Porém, não é o caso de InviteEmail, uma vez que ele lança uma exceção NotImplementedError e o body retorna um hash.

Subclasses devem manter o comportamento similar ao da classe base. Quando sobrescrevemos o método body, para retornar um hash ao invés de string, estamos quebrando o comportamento esperado. Assim, classes que esperam que o método body retorne uma string, quebram.

O mesmo problema ocorre com o método signature. Apesar da classe base possuir signature, a classe InviteEmail não implementa mais esse comportamento.

Quando esse cenário ocorrer, talvez seja melhor revisar a abstração e reavaliar o que é comportamento que deve ser herdado e o que é extensão (funcionalidade) para as classes.

Considerações Finais
O LSP tem como objetivo manter o funcionamento do código íntegro no processo de acoplamento de funcionalidades na aplicação. Esse princípio é quebrado em situações nas quais uma subclasse deixa de herdar um comportamento da classe pai, seja sobrescrevendo um método e lançando uma exceção ou não tirando proveito de todas as funcionalidades dela. Chamamos esse cenário de Refused Bequest.
