Após tiver criado o ouvinte do grupo de disponibilidade, é possível que seja necessário ajustar os parâmetros do cluster **RegisterAllProvidersIP** e **HostRecordTTL** para o recurso do ouvinte.  Esses parâmetros podem reduzir o tempo de reconexão após um failover que pode impedir as interrupções de conexão. Para obter mais informações sobre esses parâmetros, bem como um código de exemplo, consulte [Criar ou configurar um ouvinte de grupo de disponibilidade](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).



<!--HONumber=Nov16_HO3-->


