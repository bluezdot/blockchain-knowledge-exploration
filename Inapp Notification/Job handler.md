
interface NotificationJob:
	actionType: NotificationActionType,
	executionTime: number,
	address?: string
	chain?: string
	

function cronCreateLastestNotifications:
	const jobs = getCurrentJob();
	const currentTimestamp = Date.now();

	for (job in jobs):
		if withdrawJob.executionTime <= currentTimestamp:
			switch (job.actionType):
				case (WITHDRAW):
					executeWithdrawJob()
				case (CLAIM):
					executeClaimJob()
				case (RECEIVE):
					executeReceiveJob()
	
	switch (job.actionType):
		case (WITHDRAW):
			createWithdrawJob()
		case (CLAIM):
			createClaimJob()
		case (RECEIVE):
			createReceiveJob()