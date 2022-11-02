USE [DSK]
GO
/****** Object:  StoredProcedure [dbo].[pogruzka]    Script Date: 01.01.2021 16:35:12 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		<Author,,Name>
-- Create date: <Create Date,,>
-- Description:	<Description,,>
-- =============================================
ALTER PROCEDURE [dbo].[pogruzka]
	-- Add the parameters for the stored procedure here
	@DATFROM datetime,
    @DATTO datetime  
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

    -- Insert statements for procedure here
	SELECT den_smeny, Nomer_Smeni,
	MAX (CASE tab_1.ID_Rec WHEN 'pog1' THEN tab_1.Secondtime ELSE 0 END) AS pog1,
	MAX (CASE tab_1.ID_Rec WHEN 'pog2' THEN tab_1.Secondtime ELSE 0 END) AS pog2,
	MAX (CASE tab_1.ID_Rec WHEN 'pog3' THEN tab_1.Secondtime ELSE 0 END) AS pog3

FROM
	(
	SELECT ID_Rec as ID_Rec, Secondtime as Secondtime, Nomer_Smeni as Nomer_Smeni,
			CONVERT(date,
						CASE
							WHEN Nomer_Smeni='2' and ((CONVERT (time, DAT, 108) >='00:00:00.001') and  (CONVERT (time, DAT, 108)<='06:59:59.999')) THEN DAT-1
							ELSE DAT
						END, 106) as den_smeny

		FROM		
				(
				SELECT  
				ID_Rec as ID_Rec, Secondtime as Secondtime, DAT as DAT,
					CASE 
						WHEN ((CONVERT (time, DAT, 108) >='07:00:00.001') and  (CONVERT (time, DAT, 108)<='18:59:59.999')) THEN '1'
						ELSE '2'
					END Nomer_Smeni
				FROM PitateliSmena
				) tab_2

		) tab_1

		WHERE  den_smeny between  @DATFROM and @DATTO

GROUP BY den_smeny, Nomer_Smeni
ORDER BY den_smeny
END