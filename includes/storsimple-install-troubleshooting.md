<!--author=alkohli last changed: 03/17/16-->

## <a name="troubleshooting-update-failures"></a>Solucionando problemas de falhas na atualização
**E se você receber uma notificação informando que as verificações de pré-atualização falharam?**

Se uma pré-verificação falhar, certifique-se de que já observou a barra de notificação detalhada na parte inferior da página. Ela explica os motivos da falha da pré-verificação. A ilustração a seguir mostra uma instância em que uma notificação aparece. Nesse caso, a verificação de integridade do controlador e verificação de integridade do componente de hardware falharam. Na seção **Status de Hardware**, você pode ver que ambos os componentes, o **Controlador 0** e o **Controlador 1**, precisam de atenção.

  ![Falha na pré-verificação](./media/storsimple-install-troubleshooting/HCS_PreUpdateCheckFailed-include.png)

Você precisará ter certeza de que ambos os controladores estejam em boas condições e online. Também será preciso ter certeza de que todos os componentes de hardware no dispositivo StorSimple sejam mostrados como em boas condições na página Manutenção. Em seguida, você pode tentar instalar as atualizações. Se não for possível corrigir os problemas de componente de hardware, você precisará contatar o Suporte da Microsoft para saber das próximas etapas.

**E se você receber uma mensagem de erro “Não foi possível instalar atualizações” e a recomendação for consultar o guia de solução de problemas de atualização para determinar a causa da falha?**

Uma causa provável para isso pode ser a falta de conectividade com os servidores Microsoft Update. Essa é uma verificação manual que precisa ser executada. Se você perder a conectividade com o servidor de atualização, o trabalho de atualização falhará. É possível verificar a conectividade usando o seguinte cmdlet na interface do Windows PowerShell do dispositivo StorSimple:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Execute o cmdlet em ambos os controladores.

Se você verificou que há conectividade e o problema continuar, contate o Suporte da Microsoft para saber das próximas etapas.

