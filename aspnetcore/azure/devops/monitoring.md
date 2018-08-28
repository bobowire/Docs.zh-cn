---
title: 使用 ASP.NET Core 和 Azure DevOps |监视和调试
author: CamSoper
description: 提供有关为托管在 Azure 中的 ASP.NET Core 应用构建 DevOps 管道的端到端指导的指南。
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/monitor
ms.openlocfilehash: c2fc88493aee04d7ea2781d17e808581e89d2082
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2018
ms.locfileid: "42909063"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="36d52-103">监视和调试</span><span class="sxs-lookup"><span data-stu-id="36d52-103">Monitor and debug</span></span>

<span data-ttu-id="36d52-104">部署应用程序和构建 DevOps 管道，务必要了解如何监视和故障排除应用程序。</span><span class="sxs-lookup"><span data-stu-id="36d52-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="36d52-105">在本部分中，将完成以下任务：</span><span class="sxs-lookup"><span data-stu-id="36d52-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="36d52-106">查找基本的监视和故障排除 Azure 门户中的数据</span><span class="sxs-lookup"><span data-stu-id="36d52-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="36d52-107">了解 Azure Monitor 如何跨所有 Azure 服务提供更深层次查看的度量值</span><span class="sxs-lookup"><span data-stu-id="36d52-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="36d52-108">使用 Application Insights 的 web 应用连接有关的应用程序分析</span><span class="sxs-lookup"><span data-stu-id="36d52-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="36d52-109">启用日志记录并了解在何处下载日志</span><span class="sxs-lookup"><span data-stu-id="36d52-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="36d52-110">Stream 实时日志</span><span class="sxs-lookup"><span data-stu-id="36d52-110">Stream logs in real time</span></span>
* <span data-ttu-id="36d52-111">了解在何处设置警报</span><span class="sxs-lookup"><span data-stu-id="36d52-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="36d52-112">了解远程调试 Azure 应用服务 web 应用。</span><span class="sxs-lookup"><span data-stu-id="36d52-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="36d52-113">基本监视和故障排除</span><span class="sxs-lookup"><span data-stu-id="36d52-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="36d52-114">在真实时间中轻松地监视应用服务 web 应用。</span><span class="sxs-lookup"><span data-stu-id="36d52-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="36d52-115">在 Azure 门户将呈现易于理解图表和图形中的指标。</span><span class="sxs-lookup"><span data-stu-id="36d52-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="36d52-116">打开[Azure 门户](https://portal.azure.com)，然后导航到*mywebapp\<unique_number\>* 应用服务。</span><span class="sxs-lookup"><span data-stu-id="36d52-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="36d52-117">**概述**选项卡显示有用"眼"的信息，包括关系图显示最近的指标。</span><span class="sxs-lookup"><span data-stu-id="36d52-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![概述面板](./media/monitoring/overview.png)

    * <span data-ttu-id="36d52-119">**Http 5xx**： 服务器端错误的计数，通常是 ASP.NET Core 代码中的异常。</span><span class="sxs-lookup"><span data-stu-id="36d52-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="36d52-120">**数据在**： 进入你的 web 应用的数据入口。</span><span class="sxs-lookup"><span data-stu-id="36d52-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="36d52-121">**输出数据量**： 数据从 web 应用出口到客户端。</span><span class="sxs-lookup"><span data-stu-id="36d52-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="36d52-122">**请求**： 计数的 HTTP 请求。</span><span class="sxs-lookup"><span data-stu-id="36d52-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="36d52-123">**平均响应时间**： 的 web 应用响应 HTTP 请求的平均时间。</span><span class="sxs-lookup"><span data-stu-id="36d52-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="36d52-124">此外在此页上找到多个自助服务工具进行故障排除和优化。</span><span class="sxs-lookup"><span data-stu-id="36d52-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![自助服务工具](./media/monitoring/wizards.png)

    * <span data-ttu-id="36d52-126">**诊断并解决问题**是自助服务的故障排除程序。</span><span class="sxs-lookup"><span data-stu-id="36d52-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="36d52-127">**Application Insights**用于分析性能和应用程序行为，稍后在本部分中讨论。</span><span class="sxs-lookup"><span data-stu-id="36d52-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="36d52-128">**应用服务顾问**会提出建议来优化您的应用体验。</span><span class="sxs-lookup"><span data-stu-id="36d52-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="36d52-129">高级监视</span><span class="sxs-lookup"><span data-stu-id="36d52-129">Advanced monitoring</span></span>

<span data-ttu-id="36d52-130">[Azure 监视器](https://docs.microsoft.com/azure/monitoring-and-diagnostics/)是用于监视所有度量值以及跨 Azure 服务中设置警报的集中式的服务。</span><span class="sxs-lookup"><span data-stu-id="36d52-130">[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="36d52-131">在 Azure Monitor，管理员可以精细地跟踪性能和确定趋势。</span><span class="sxs-lookup"><span data-stu-id="36d52-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="36d52-132">每个 Azure 服务提供其自己[度量值组](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)到 Azure Monitor。</span><span class="sxs-lookup"><span data-stu-id="36d52-132">Each Azure service offers its own [set of metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="36d52-133">使用 Application Insights 配置文件</span><span class="sxs-lookup"><span data-stu-id="36d52-133">Profile with Application Insights</span></span>

<span data-ttu-id="36d52-134">[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview)是用于分析的性能和稳定性的 web 应用和用户如何使用这些 Azure 服务。</span><span class="sxs-lookup"><span data-stu-id="36d52-134">[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="36d52-135">从 Application Insights 的数据是更广泛的和更深入地与 Azure Monitor。</span><span class="sxs-lookup"><span data-stu-id="36d52-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="36d52-136">开发人员和管理员使用的信息用于改进应用，可以提供数据。</span><span class="sxs-lookup"><span data-stu-id="36d52-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="36d52-137">可以将 application Insights 添加到 Azure 应用服务资源无需更改代码。</span><span class="sxs-lookup"><span data-stu-id="36d52-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="36d52-138">打开[Azure 门户](https://portal.azure.com)，然后导航到*mywebapp\<unique_number\>* 应用服务。</span><span class="sxs-lookup"><span data-stu-id="36d52-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="36d52-139">从**概述**选项卡上，单击**Application Insights**磁贴。</span><span class="sxs-lookup"><span data-stu-id="36d52-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Application Insights 磁贴](./media/monitoring/app-insights.png)

1. <span data-ttu-id="36d52-141">选择**创建新的资源**单选按钮。</span><span class="sxs-lookup"><span data-stu-id="36d52-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="36d52-142">使用默认资源名称，并选择 Application Insights 资源的位置。</span><span class="sxs-lookup"><span data-stu-id="36d52-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="36d52-143">位置不需要你的 web 应用的相匹配。</span><span class="sxs-lookup"><span data-stu-id="36d52-143">The location doesn't need to match that of your web app.</span></span>

    ![Application Insights 安装](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="36d52-145">有关**运行时/框架**，选择**ASP.NET Core**。</span><span class="sxs-lookup"><span data-stu-id="36d52-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="36d52-146">接受默认设置。</span><span class="sxs-lookup"><span data-stu-id="36d52-146">Accept the default settings.</span></span>
1. <span data-ttu-id="36d52-147">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="36d52-147">Select **OK**.</span></span> <span data-ttu-id="36d52-148">如果系统提示你确认，请选择**继续**。</span><span class="sxs-lookup"><span data-stu-id="36d52-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="36d52-149">创建资源后，单击要直接导航到 Application Insights 页的 Application Insights 资源的名称。</span><span class="sxs-lookup"><span data-stu-id="36d52-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![新的 Application Insights 资源是准备就绪](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="36d52-151">使用应用程序时，数据累积。</span><span class="sxs-lookup"><span data-stu-id="36d52-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="36d52-152">选择**刷新**重新加载新数据的边栏选项卡。</span><span class="sxs-lookup"><span data-stu-id="36d52-152">Select **Refresh** to reload the blade with new data.</span></span>

![Application Insights 概述选项卡](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="36d52-154">Application Insights 提供有用的服务器端的信息，而无需其他配置。</span><span class="sxs-lookup"><span data-stu-id="36d52-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="36d52-155">若要从 Application Insights，获取最大的价值[使用 Application Insights SDK 检测应用](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="36d52-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="36d52-156">如果配置正确，该服务提供端对端监控跨 web 服务器和浏览器中，包括客户端的性能。</span><span class="sxs-lookup"><span data-stu-id="36d52-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="36d52-157">有关详细信息，请参阅[Application Insights 文档](https://docs.microsoft.com/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="36d52-157">For more information, see the [Application Insights documentation](https://docs.microsoft.com/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="36d52-158">日志记录</span><span class="sxs-lookup"><span data-stu-id="36d52-158">Logging</span></span>

<span data-ttu-id="36d52-159">在 Azure 应用服务中的默认情况下禁用 web 服务器和应用程序日志。</span><span class="sxs-lookup"><span data-stu-id="36d52-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="36d52-160">启用日志通过以下步骤：</span><span class="sxs-lookup"><span data-stu-id="36d52-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="36d52-161">打开[Azure 门户](https://portal.azure.com)，并导航到*mywebapp\<unique_number\>* 应用服务。</span><span class="sxs-lookup"><span data-stu-id="36d52-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="36d52-162">在左侧菜单中，向下滚动到**监视**部分。</span><span class="sxs-lookup"><span data-stu-id="36d52-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="36d52-163">选择**诊断日志**。</span><span class="sxs-lookup"><span data-stu-id="36d52-163">Select **Diagnostics logs**.</span></span>

    ![诊断日志链接](./media/monitoring/logging.png)

1. <span data-ttu-id="36d52-165">开启**应用程序日志记录 （文件系统）**。</span><span class="sxs-lookup"><span data-stu-id="36d52-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="36d52-166">如果系统提示，请单击此框来安装扩展，使应用程序中 web 应用的日志记录。</span><span class="sxs-lookup"><span data-stu-id="36d52-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="36d52-167">设置**Web 服务器日志记录**到**文件系统**。</span><span class="sxs-lookup"><span data-stu-id="36d52-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="36d52-168">输入**保留期**以天为单位。</span><span class="sxs-lookup"><span data-stu-id="36d52-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="36d52-169">例如，30。</span><span class="sxs-lookup"><span data-stu-id="36d52-169">For example, 30.</span></span>
1. <span data-ttu-id="36d52-170">单击“保存” 。</span><span class="sxs-lookup"><span data-stu-id="36d52-170">Click **Save**.</span></span>

<span data-ttu-id="36d52-171">为 web 应用生成 ASP.NET Core 和 web 服务器 （应用服务） 日志。</span><span class="sxs-lookup"><span data-stu-id="36d52-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="36d52-172">可以使用显示的 FTP/FTPS 信息下载它们。</span><span class="sxs-lookup"><span data-stu-id="36d52-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="36d52-173">密码是之前在本指南中创建的部署凭据相同。</span><span class="sxs-lookup"><span data-stu-id="36d52-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="36d52-174">日志可能很[流式传输到使用 PowerShell 或 Azure CLI 在本地计算机直接](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download)。</span><span class="sxs-lookup"><span data-stu-id="36d52-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="36d52-175">也可以是日志[Application Insights 中查看](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)。</span><span class="sxs-lookup"><span data-stu-id="36d52-175">Logs can also be [viewed in Application Insights](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="36d52-176">日志流式处理</span><span class="sxs-lookup"><span data-stu-id="36d52-176">Log streaming</span></span>

<span data-ttu-id="36d52-177">可以通过门户实时传输应用程序和 web 服务器日志。</span><span class="sxs-lookup"><span data-stu-id="36d52-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="36d52-178">打开[Azure 门户](https://portal.azure.com)，并导航到*mywebapp\<unique_number\>* 应用服务。</span><span class="sxs-lookup"><span data-stu-id="36d52-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="36d52-179">在左侧菜单中，向下滚动到**监视**部分，并选择**日志流**。</span><span class="sxs-lookup"><span data-stu-id="36d52-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![日志流链接](./media/monitoring/log-stream.png)

<span data-ttu-id="36d52-181">也可以是日志[流式处理通过 Azure CLI 或 Azure PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)，其中包括通过 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="36d52-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="36d52-182">警报</span><span class="sxs-lookup"><span data-stu-id="36d52-182">Alerts</span></span>

<span data-ttu-id="36d52-183">Azure 监视器还提供[实时警报](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)基于度量值、 管理事件和其他条件。</span><span class="sxs-lookup"><span data-stu-id="36d52-183">Azure Monitor also provides [real time alerts](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="36d52-184">*注意： 当前在 web 应用度量值均可收到警报仅在警报 （经典） 服务中。*</span><span class="sxs-lookup"><span data-stu-id="36d52-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="36d52-185">[警报 （经典） 服务](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)在 Azure Monitor 或下可以找到**监视**部分中的应用服务设置。</span><span class="sxs-lookup"><span data-stu-id="36d52-185">The [Alerts (classic) service](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![警报 （经典） 链接](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="36d52-187">实时调试</span><span class="sxs-lookup"><span data-stu-id="36d52-187">Live debugging</span></span>

<span data-ttu-id="36d52-188">Azure 应用服务可以是[使用 Visual Studio 远程调试](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)日志时不提供足够的信息。</span><span class="sxs-lookup"><span data-stu-id="36d52-188">Azure App Service can be [debugged remotely with Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="36d52-189">但是，远程调试要求应用程序以使用调试符号编译。</span><span class="sxs-lookup"><span data-stu-id="36d52-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="36d52-190">调试不应执行在生产中，除了作为最后的手段。</span><span class="sxs-lookup"><span data-stu-id="36d52-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="36d52-191">结束语</span><span class="sxs-lookup"><span data-stu-id="36d52-191">Conclusion</span></span>

<span data-ttu-id="36d52-192">在本部分中，您将完成以下任务：</span><span class="sxs-lookup"><span data-stu-id="36d52-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="36d52-193">查找基本的监视和故障排除 Azure 门户中的数据</span><span class="sxs-lookup"><span data-stu-id="36d52-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="36d52-194">了解 Azure Monitor 如何跨所有 Azure 服务提供更深层次查看的度量值</span><span class="sxs-lookup"><span data-stu-id="36d52-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="36d52-195">使用 Application Insights 的 web 应用连接有关的应用程序分析</span><span class="sxs-lookup"><span data-stu-id="36d52-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="36d52-196">启用日志记录并了解在何处下载日志</span><span class="sxs-lookup"><span data-stu-id="36d52-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="36d52-197">Stream 实时日志</span><span class="sxs-lookup"><span data-stu-id="36d52-197">Stream logs in real time</span></span>
* <span data-ttu-id="36d52-198">了解在何处设置警报</span><span class="sxs-lookup"><span data-stu-id="36d52-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="36d52-199">了解远程调试 Azure 应用服务 web 应用。</span><span class="sxs-lookup"><span data-stu-id="36d52-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="36d52-200">其他阅读材料</span><span class="sxs-lookup"><span data-stu-id="36d52-200">Additional reading</span></span>

* [<span data-ttu-id="36d52-201">对 Azure 应用服务上的 ASP.NET Core 进行故障排除</span><span class="sxs-lookup"><span data-stu-id="36d52-201">Troubleshoot ASP.NET Core on Azure App Service</span></span>](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="36d52-202">Azure 应用服务和 IIS 上 ASP.NET Core 的常见错误参考</span><span class="sxs-lookup"><span data-stu-id="36d52-202">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="36d52-203">监视使用 Application Insights 的 Azure web 应用性能</span><span class="sxs-lookup"><span data-stu-id="36d52-203">Monitor Azure web app performance with Application Insights</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="36d52-204">在 Azure 应用服务中为应用启用诊断日志记录</span><span class="sxs-lookup"><span data-stu-id="36d52-204">Enable diagnostics logging for web apps in Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="36d52-205">使用 Visual Studio 对 Azure 应用服务中的 Web 应用进行故障排除</span><span class="sxs-lookup"><span data-stu-id="36d52-205">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="36d52-206">Azure Monitor 中创建经典指标警报的 Azure 服务-Azure 门户</span><span class="sxs-lookup"><span data-stu-id="36d52-206">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)