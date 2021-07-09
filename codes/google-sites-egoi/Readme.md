## Código para inserir e formatar formulário do E-goi

### Código de estilo para o formulário

```CSS
<style>
  .cardEmail{
	  background-color:#EEFFFF;
	  padding: 20px;
	  border: 1px solid #22aaff;
	  border-radius: 8px;
	  box-shadow: 0 0 10px #85888C;
    max-width: 600px;
    min-width: 200px;
    margin:auto;
}
  .cardEmail > form{
	  display: flex;
	  flex-flow: column wrap;
}
  .cardEmail > form > h3{
    text-align: center;
}
  .cardEmail > form > p{
    text-align: center;
}
  .cardEmail > form > hr{
    background-color:#ccc;
  }  
  .cardEmail > form > #email_6{
	  border-radius:6px;
	  border: 1px solid #22aaff;
	  width: 100%;
	  height: 35px;
    text-align: center;
    font-size:clamp(0.5rem,  0.5rem + 2.5vw, 4rem);
  
}
  .cardEmail > form > input[type=submit]{
	  background-color:#EB224B;
	  color: #fff;
	  height: 50px;
	  width: 102%;
	  font-size:clamp(0.5rem, 0.5rem + 2.5vw, 4rem);
	  border: 1px solid #962757;
	  border-radius:8px;
    margin-top:8px ;
    cursor: pointer;
}
  .cardEmail > form > input[type=submit]:hover{
    background-color:#5D9E18;
    border: 1px solid #70FF70;
  }
</style>
```

### Código Html para inserir o formulário

```HTML
<div class="cardEmail">
	<form method="post" enctype="multipart/form-data" action="https://28.e-goi.com/w/1e3e1HaieQ5plG0GVe9fddfa9-">
		<input type="hidden" name="lista" value="1">
  	<input type="hidden" name="cliente" value="364217">
  	<input type="hidden" name="lang" id="lang_id" value="br">
  	<input type="hidden" name="formid" id="formid" value="3">
		<h3>Baixe gratuitamente este E-book!</h3>
		<p>Coloque seu melhor e-mail para receber o link de acesso ao e-book.</p>
  	<input type="email" name="email_6" id="email_6" value="" data-np-checked="1" placeholder="Seu melhor e-mail aqui" required>
  	<input type="submit" value="Quero este E-book">	
	</form>
</div>
```


