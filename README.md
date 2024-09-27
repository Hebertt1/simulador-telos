# simulador-telos
Master page 
<%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site.master.cs" Inherits="SimuladorSeguro.SiteMaster" %>

<!DOCTYPE html>
<html lang="pt-br">
<head runat="server">
    <meta charset="utf-8" />
    <title>Simulador de Seguro</title>
    <link href="~/Content/Site.css" rel="stylesheet" />
</head>
<body>
    <form id="form1" runat="server">
        <div>
            <header>
                <h1>Simulador de Seguro</h1>
            </header>

            <nav>
                <ul>
                    <li><a href="Default.aspx">Início</a></li>
                    <li><a href="Form.aspx">Simular Seguro</a></li>
                </ul>
            </nav>

            <asp:ContentPlaceHolder ID="MainContent" runat="server">
            </asp:ContentPlaceHolder>

            <footer>
                <p>&copy; 2024 - Simulador de Seguro</p>
            </footer>
        </div>
    </form>
</body>
</html>
Form Page
<%@ Page Title="Simulação de Seguro" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Form.aspx.cs" Inherits="SimuladorSeguro.Form" %>

<asp:Content ID="Content1" ContentPlaceHolderID="MainContent" runat="server">
    <h2>Simule o seu Seguro</h2>
    
    <asp:Label ID="lblNome" runat="server" Text="Nome:"></asp:Label>
    <asp:TextBox ID="txtNome" runat="server"></asp:TextBox>
    <br />
    
    <asp:Label ID="lblNascimento" runat="server" Text="Data de Nascimento:"></asp:Label>
    <asp:TextBox ID="txtNascimento" runat="server"></asp:TextBox>
    <asp:CalendarExtender TargetControlID="txtNascimento" runat="server" />
    <br />
    
    <asp:Label ID="lblCPF" runat="server" Text="CPF:"></asp:Label>
    <asp:TextBox ID="txtCPF" runat="server"></asp:TextBox>
    <br />
    
    <asp:Label ID="lblSeguro" runat="server" Text="Tipo de Seguro:"></asp:Label>
    <asp:DropDownList ID="ddlSeguro" runat="server">
        <asp:ListItem Value="1">Seguro de Vida</asp:ListItem>
        <asp:ListItem Value="2">Seguro de Morte Acidental</asp:ListItem>
        <asp:ListItem Value="3">Seguro Contra Acidentes Pessoais</asp:ListItem>
        <asp:ListItem Value="4">Seguro de Saúde</asp:ListItem>
        <asp:ListItem Value="5">Seguro de Automóvel</asp:ListItem>
        <asp:ListItem Value="6">Seguro Residencial</asp:ListItem>
        <asp:ListItem Value="7">Seguro Patrimonial</asp:ListItem>
        <asp:ListItem Value="8">Seguro Empresarial</asp:ListItem>
    </asp:DropDownList>
    <br />
    
    <asp:Button ID="btnCalcular" runat="server" Text="Calcular Seguro" OnClick="btnCalcular_Click" />
</asp:Content>
Código-Behind
protected void btnCalcular_Click(object sender, EventArgs e)
{
    string nome = txtNome.Text;
    DateTime nascimento = DateTime.Parse(txtNascimento.Text);
    string cpf = txtCPF.Text;
    int tipoSeguro = int.Parse(ddlSeguro.SelectedValue);

    double valorBase = 1000; // Valor base para todos os seguros
    double valorSeguro = 0;

    switch (tipoSeguro)
    {
        case 1:
            valorSeguro = valorBase * 0.80; // 80% para Seguro de Vida
            break;
        case 2:
            valorSeguro = valorBase * 0.90; // 90% para Seguro de Morte Acidental
            break;
        case 3:
            valorSeguro = valorBase * 0.50; // 50% para Acidentes Pessoais
            break;
        case 4:
            valorSeguro = valorBase * 0.40; // 40% para Seguro de Saúde
            break;
        case 5:
            valorSeguro = valorBase * 2.50; // 250% para Seguro de Automóvel
            break;
        case 6:
            valorSeguro = valorBase * 0.60; // 60% para Seguro Residencial
            break;
        case 7:
            valorSeguro = valorBase * 1.20; // 120% para Seguro Patrimonial
            break;
        case 8:
            valorSeguro = valorBase * 1.50; // 150% para Seguro Empresarial
            break;
    }

    // Redireciona para a página de resultado, passando os dados
    Response.Redirect($"Resultado.aspx?nome={nome}&nascimento={nascimento}&cpf={cpf}&valor={valorSeguro}");
}
Página de Resultado
<%@ Page Title="Resultado da Simulação" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Resultado.aspx.cs" Inherits="SimuladorSeguro.Resultado" %>

<asp:Content ID="Content1" ContentPlaceHolderID="MainContent" runat="server">
    <h2>Resultado da Simulação</h2>
    
    <p>Nome: <asp:Label ID="lblNome" runat="server"></asp:Label></p>
    <p>Data de Nascimento: <asp:Label ID="lblNascimento" runat="server"></asp:Label></p>
    <p>CPF: <asp:Label ID="lblCPF" runat="server"></asp:Label></p>
    <p>Valor do Seguro: <asp:Label ID="lblValor" runat="server"></asp:Label></p>

    <asp:Button ID="btnVoltar" runat="server" Text="Voltar" OnClick="btnVoltar_Click" />
</asp:Content>
 Código-Behind da Página de Resultado
 protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        lblNome.Text = Request.QueryString["nome"];
        lblNascimento.Text = Request.QueryString["nascimento"];
        lblCPF.Text = Request.QueryString["cpf"];
        lblValor.Text = Request.QueryString["valor"];
    }
}

protected void btnVoltar_Click(object sender, EventArgs e)
{
    Response.Redirect("Form.aspx");
}

