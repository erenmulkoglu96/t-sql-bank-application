Create Database BankaDb
Go
Use BankaDb
Go
Create Table ABanka
(
HesapNo int,
Bakiye money
)
Go
Create Table BBanka
(
HesapNo int,
Bakiye money
)
Go
Insert ABanka Values(10, 1000),
					(20, 2500)
Insert BBanka Values(30, 2300),
					(40, 760)
Go
Create Proc sp_HavaleYap
(
@BankaKimden nvarchar(MAX),
@GonderenHesapNo int,
@AlanHesapNo int,
@Tutar money
)
As

Begin Transaction Kontrol
Declare @ABakiye int, @BBakiye int, @HesaptakiPara money
If @BankaKimden = 'ABanka'
Begin
	Select @HesaptakiPara = Bakiye from ABanka Where HesapNo = @GonderenHesapNo
	If @Tutar > @HesaptakiPara
	Begin
		print CAST(@GonderenHesapNo as nvarchar(MAX)) + ' numaralı hesapta, gönderilmek istenen tutardan az para mevcuttur.' 
		Rollback -- İşlemleri Geri Al
	End
	Else
	Begin
		Update ABanka Set Bakiye = Bakiye - @Tutar Where HesapNo = @GonderenHesapNo
		Update BBanka Set Bakiye = Bakiye + @Tutar Where HesapNo = @AlanHesapNo
		print 'ABankasındaki ' + CAST(@GonderenHesapNo as nvarchar(MAX)) + ' numaralı hesaptan BBankasındaki ' + CAST(@AlanHesapNo as nvarchar(MAX)) + ' numaralı hesaba ' + CAST(@Tutar as nvarchar(MAX)) + ' değerinde para havale edilmiştir.'
		print 'Son değerler;'

		Select @ABakiye = Bakiye from ABanka Where HesapNo = @GonderenHesapNo
		Select @BBakiye = Bakiye from BBanka Where HesapNo = @AlanHesapNo
		print 'ABankasındaki ' + CAST(@GonderenHesapNo as nvarchar(MAX)) + ' numaralı hesapta kalan bakiye : ' + CAST(@ABakiye as nvarchar(MAX))
		print 'BBankasındaki ' + CAST(@AlanHesapNo as nvarchar(MAX)) + ' numaralı hesapta kalan bakiye : ' + CAST(@BBakiye as nvarchar(MAX))
		Commit
	End
End
Else
Begin
	Select @HesaptakiPara = Bakiye from BBanka Where HesapNo = @GonderenHesapNo
	If @Tutar > @HesaptakiPara
	Begin
		print CAST(@GonderenHesapNo as nvarchar(MAX)) + ' numaralı hesapta, gönderilmek istenen tutardan az para mevcuttur.'
		Rollback -- İşlemleri Geri Al
	End
	Else
	Begin
		Update BBanka Set Bakiye = Bakiye - @Tutar Where HesapNo = @GonderenHesapNo
		Update ABanka Set Bakiye = Bakiye + @Tutar Where HesapNo = @AlanHesapNo
		print 'BBankasındaki ' + CAST(@GonderenHesapNo as nvarchar(MAX)) + ' numaralı hesaptan ABankasındaki ' + CAST(@AlanHesapNo as nvarchar(MAX)) + ' numaralı hesaba' + CAST(@Tutar as nvarchar(MAX))+ ' değerinde para havale edilmiştir.'
		print 'Son Değerler;'

		Select @BBakiye = Bakiye from BBanka Where HesapNo = @GonderenHesapNo
		Select @ABakiye = Bakiye from ABanka Where HesapNo = @AlanHesapNo
		print 'ABankasındaki ' + CAST(@AlanHesapNo as nvarchar(MAX)) + ' numaralı hesapta kalan bakye : ' + CAST(@ABakiye as nvarchar(MAX))
		print 'BBankasındaki ' + CAST(@GonderenHesapNo as nvarchar(MAX)) + ' numaralı hesapta kalan bakiye : ' + CAST(@BBakiye as nvarchar(MAX))
		Commit
	End
End

Exec sp_HavaleYap 'ABanka', 10,30, 100
Exec sp_HavaleYap 'BBanka', 30, 10, 300
