---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 de teste de unidade | Microsoft Docs
author: tfitzmac
description: Este guia e o aplicativo demonstram como criar testes de unidade simples para seu aplicativo de API Web 2. Este tutorial mostra como incluir um projeto de teste de unidade...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="c077f-104">ASP.NET Web API 2 de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="c077f-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c077f-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c077f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="c077f-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c077f-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="c077f-107">Este guia e o aplicativo demonstram como criar testes de unidade simples para seu aplicativo de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="c077f-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="c077f-108">Este tutorial mostra como incluir um projeto de teste de unidade em sua solução e escrever os métodos de teste que verificam os valores retornados de um método do controlador.</span><span class="sxs-lookup"><span data-stu-id="c077f-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="c077f-109">Este tutorial pressupõe que você esteja familiarizado com os conceitos básicos da API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c077f-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="c077f-110">Para um tutorial de Introdução, consulte [guia de Introdução ao ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c077f-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="c077f-111">Os testes de unidade neste tópico são intencionalmente limitados aos cenários de dados simples.</span><span class="sxs-lookup"><span data-stu-id="c077f-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="c077f-112">Para cenários mais avançados de dados de teste de unidade, consulte [fictícias Entity Framework quando a unidade de teste ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="c077f-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c077f-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="c077f-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="c077f-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c077f-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="c077f-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c077f-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="c077f-116">Neste tópico</span><span class="sxs-lookup"><span data-stu-id="c077f-116">In this topic</span></span>

<span data-ttu-id="c077f-117">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="c077f-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="c077f-118">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c077f-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="c077f-119">Baixar o código</span><span class="sxs-lookup"><span data-stu-id="c077f-119">Download code</span></span>](#download)
- [<span data-ttu-id="c077f-120">Criar aplicativo com o projeto de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="c077f-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="c077f-121">Adicionar projeto de teste de unidade ao criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c077f-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="c077f-122">Adicionar projeto de teste de unidade para um aplicativo existente</span><span class="sxs-lookup"><span data-stu-id="c077f-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="c077f-123">Configurar o aplicativo de API Web 2</span><span class="sxs-lookup"><span data-stu-id="c077f-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="c077f-124">Instalar os pacotes do NuGet no projeto de teste</span><span class="sxs-lookup"><span data-stu-id="c077f-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="c077f-125">Criar testes</span><span class="sxs-lookup"><span data-stu-id="c077f-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="c077f-126">Executar testes</span><span class="sxs-lookup"><span data-stu-id="c077f-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="c077f-127">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c077f-127">Prerequisites</span></span>

<span data-ttu-id="c077f-128">Edição do Visual Studio 2017 Community, Professional ou Enterprise</span><span class="sxs-lookup"><span data-stu-id="c077f-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="c077f-129">Baixar o código</span><span class="sxs-lookup"><span data-stu-id="c077f-129">Download code</span></span>

<span data-ttu-id="c077f-130">Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="c077f-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="c077f-131">O projeto para download inclui o código de teste de unidade para este tópico e o [fictícias Entity Framework quando API de Web do ASP.NET de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tópico.</span><span class="sxs-lookup"><span data-stu-id="c077f-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="c077f-132">Criar aplicativo com o projeto de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="c077f-132">Create application with unit test project</span></span>

<span data-ttu-id="c077f-133">Você pode criar um projeto de teste de unidade durante a criação de seu aplicativo ou adicionar um projeto de teste de unidade para um aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="c077f-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="c077f-134">Este tutorial mostra os dois métodos para criar um projeto de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="c077f-135">Para seguir este tutorial, você pode usar qualquer abordagem.</span><span class="sxs-lookup"><span data-stu-id="c077f-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="c077f-136">Adicionar projeto de teste de unidade ao criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c077f-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="c077f-137">Criar um novo aplicativo de Web do ASP.NET chamado **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="c077f-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Criar projeto](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="c077f-139">Nas janelas do novo projeto ASP.NET, selecione o **vazio** modelo e adicionar pastas e referências de núcleo para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="c077f-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="c077f-140">Selecione o **adicionar testes de unidade** opção.</span><span class="sxs-lookup"><span data-stu-id="c077f-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="c077f-141">O projeto de teste de unidade é automaticamente denominado **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="c077f-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="c077f-142">Você pode manter esse nome.</span><span class="sxs-lookup"><span data-stu-id="c077f-142">You can keep this name.</span></span>

![Criar projeto de teste de unidade](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="c077f-144">Depois de criar o aplicativo, você verá que ele contém dois projetos.</span><span class="sxs-lookup"><span data-stu-id="c077f-144">After creating the application, you will see it contains two projects.</span></span>

![dois projetos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="c077f-146">Adicionar projeto de teste de unidade para um aplicativo existente</span><span class="sxs-lookup"><span data-stu-id="c077f-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="c077f-147">Se você não criou o projeto de teste de unidade ao criar seu aplicativo, você pode adicionar um a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="c077f-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="c077f-148">Por exemplo, suponha que você já tiver um aplicativo chamado StoreApp, e você deseja adicionar testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="c077f-149">Para adicionar um projeto de teste de unidade, clique em sua solução e selecione **adicionar** e **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="c077f-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Adicionar novo projeto à solução](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="c077f-151">Selecione **teste** no painel esquerdo e selecione **projeto de teste de unidade** para o tipo de projeto.</span><span class="sxs-lookup"><span data-stu-id="c077f-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="c077f-152">Nomeie o projeto **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="c077f-152">Name the project **StoreApp.Tests**.</span></span>

![Adicionar projeto de teste de unidade](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="c077f-154">Você verá o projeto de teste de unidade em sua solução.</span><span class="sxs-lookup"><span data-stu-id="c077f-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="c077f-155">No projeto de teste de unidade, adicione uma referência de projeto para o projeto original.</span><span class="sxs-lookup"><span data-stu-id="c077f-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="c077f-156">Configurar o aplicativo de API Web 2</span><span class="sxs-lookup"><span data-stu-id="c077f-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="c077f-157">No seu projeto de StoreApp, adicione um arquivo de classe para o **modelos** pasta denominada **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="c077f-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="c077f-158">Substitua o conteúdo do arquivo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c077f-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="c077f-159">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="c077f-159">Build the solution.</span></span>

<span data-ttu-id="c077f-160">Clique na pasta controladores e selecione **adicionar** e **Novo Item de Scaffold**.</span><span class="sxs-lookup"><span data-stu-id="c077f-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="c077f-161">Selecione **Web API 2 controlador - vazio**.</span><span class="sxs-lookup"><span data-stu-id="c077f-161">Select **Web API 2 Controller - Empty**.</span></span>

![Adicionar novo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="c077f-163">Defina o nome do controlador **SimpleProductController**e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c077f-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Especifique o controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="c077f-165">Substitua o código existente pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c077f-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="c077f-166">Para simplificar este exemplo, os dados são armazenados em uma lista em vez de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c077f-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="c077f-167">A lista definida nesta classe representa os dados de produção.</span><span class="sxs-lookup"><span data-stu-id="c077f-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="c077f-168">Observe que o controlador inclui um construtor que aceita como um parâmetro de uma lista de objetos do produto.</span><span class="sxs-lookup"><span data-stu-id="c077f-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="c077f-169">Este construtor permite passar dados de teste ao teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="c077f-170">O controlador também inclui duas **async** métodos para ilustrar os métodos assíncronos de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="c077f-171">Esses métodos assíncronos foram implementados chamando **Task.FromResult** minimizar estranhos código, mas normalmente os métodos inclui operações com uso intensivo de recursos.</span><span class="sxs-lookup"><span data-stu-id="c077f-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="c077f-172">O método GetProduct retorna uma instância do **IHttpActionResult** interface.</span><span class="sxs-lookup"><span data-stu-id="c077f-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="c077f-173">IHttpActionResult é um dos novos recursos no API Web 2, e ele simplifica o desenvolvimento de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="c077f-174">Classes que implementam a interface IHttpActionResult são encontradas no [Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span><span class="sxs-lookup"><span data-stu-id="c077f-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="c077f-175">Essas classes representam as respostas possíveis de uma solicitação de ação e eles correspondem aos códigos de status HTTP.</span><span class="sxs-lookup"><span data-stu-id="c077f-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="c077f-176">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="c077f-176">Build the solution.</span></span>

<span data-ttu-id="c077f-177">Agora você está pronto para configurar o projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="c077f-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="c077f-178">Instalar os pacotes do NuGet no projeto de teste</span><span class="sxs-lookup"><span data-stu-id="c077f-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="c077f-179">Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp.Tests) não inclui todos os pacotes NuGet instalados.</span><span class="sxs-lookup"><span data-stu-id="c077f-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="c077f-180">Outros modelos, como o modelo de API da Web, incluem alguns pacotes do NuGet no projeto de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="c077f-181">Para este tutorial, você deve incluir o pacote do Microsoft ASP.NET Web API 2 Core para o projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="c077f-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="c077f-182">O projeto StoreApp.Tests e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c077f-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="c077f-183">Você deve selecionar o projeto StoreApp.Tests para adicionar os pacotes ao projeto.</span><span class="sxs-lookup"><span data-stu-id="c077f-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Gerenciar pacotes](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="c077f-185">Localizar e instalar o pacote do Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="c077f-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalar o pacote de núcleo de api da web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="c077f-187">Feche a janela Gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="c077f-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="c077f-188">Criar testes</span><span class="sxs-lookup"><span data-stu-id="c077f-188">Create tests</span></span>

<span data-ttu-id="c077f-189">Por padrão, o projeto de teste inclui um arquivo de teste vazio chamado UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="c077f-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="c077f-190">Esse arquivo mostra os atributos que você usa para criar métodos de teste.</span><span class="sxs-lookup"><span data-stu-id="c077f-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="c077f-191">Para testes de unidade, você pode usar esse arquivo ou criar seu próprio arquivo.</span><span class="sxs-lookup"><span data-stu-id="c077f-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="c077f-193">Para este tutorial, você criará sua própria classe de teste.</span><span class="sxs-lookup"><span data-stu-id="c077f-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="c077f-194">Você pode excluir o arquivo UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="c077f-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="c077f-195">Adicione uma classe denominada **TestSimpleProductController.cs**e substitua o código com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c077f-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="c077f-196">Executar testes</span><span class="sxs-lookup"><span data-stu-id="c077f-196">Run tests</span></span>

<span data-ttu-id="c077f-197">Agora você está pronto para executar os testes.</span><span class="sxs-lookup"><span data-stu-id="c077f-197">You are now ready to run the tests.</span></span> <span data-ttu-id="c077f-198">Todo o método que são marcados com o **TestMethod** atributo será testado.</span><span class="sxs-lookup"><span data-stu-id="c077f-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="c077f-199">Do **teste** item de menu, execute os testes.</span><span class="sxs-lookup"><span data-stu-id="c077f-199">From the **Test** menu item, run the tests.</span></span>

![executar testes](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="c077f-201">Abra o **Gerenciador de testes** janela e observe os resultados dos testes.</span><span class="sxs-lookup"><span data-stu-id="c077f-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de testes](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="c077f-203">Resumo</span><span class="sxs-lookup"><span data-stu-id="c077f-203">Summary</span></span>

<span data-ttu-id="c077f-204">Você concluiu este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c077f-204">You have completed this tutorial.</span></span> <span data-ttu-id="c077f-205">Os dados neste tutorial intencionalmente foi simplificados para focalizar as condições de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c077f-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="c077f-206">Para cenários mais avançados de dados de teste de unidade, consulte [fictícias Entity Framework quando a unidade de teste ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="c077f-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>