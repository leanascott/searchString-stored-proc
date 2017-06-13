# searchString-stored-proc

ALTER PROCEDURE [dbo].[Squad_Search]

    @SearchStr nvarchar(100) = ''
	

AS
BEGIN
/* test code
	

	EXEC Squad_Search '' 

*/
	
	
	SELECT Id
		, Name
		, Mission
		, Notes
	INTO #RESULT
	FROM Squad
	WHERE (Name LIKE '%' + @SearchStr + '%'
	or Mission like '%' + @SearchStr + '%')
	ORDER BY Name, Mission

	

	SELECT * FROM #RESULT ORDER BY Name, Mission

	
	SELECT se.Id
		, se.Name
		, se.DateCreated
		, se.DateModified
		, se.UserIdCreated
		, se.EventStart
		, se.EventEnd
		, se.Description
		, se.Location
		, se.SquadId
		, se.Color
		, se.Timezone
	, s.Name AS SquadName
	FROM SquadEvent se
		 JOIN #RESULT s
			ON se.SquadId = s.Id
	
	

	SELECT	sm.Id
		, sm.MemberStatusId
		, sm.SquadId
		, sm.PersonId
		, sm.LeaderComment
		, sm.DateCreated
		, sm.DateModified
		, p.FirstName
		, p.MiddleName
		, p.LastName
		, p.PhoneNumber
		, p.Email
	FROM SquadMember sm
		Join Person p
			on sm.PersonId = p.Id
		JOIN #RESULT s 
			on sm.SquadId = s.Id

    
END
