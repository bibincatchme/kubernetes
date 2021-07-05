aws efs create-mount-target --file-system-id fs-216505 --subnet-id subnet-017ef261df8ea --security-group sg-018e76160480 --region us-east-1

	 
aws efs create-access-point --file-system-id fs-0a660 --posix-user Uid=1000,Gid=1000 --root-directory "Path=/jenkins,CreationInfo={OwnerUid=1000,OwnerGid=1000,Permissions=777}"
