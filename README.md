## ��������� �������������� � ������� BORGBACKUP
����� ������� �� ���� �������� ������� (192.168.11.103) � ������� ������� (192.168.11.102).
�� ����� ������� ������ ������������ borg � � ������� ������� ������� ��������������� id_rsa.pub ���� ��� �������������� ��� ������ �� ssh.
�� ����� �������� ��������� �������� ���� borg � ����� ����� �� ���������� ������������ borg. 
�� ������� ������� ������ ����������  /Repository ��� �������� �����������. � ���������� ������ �������� �������� �������� 

	/usr/local/bin/borg init -e none borg@192.168.11.102

�� ���������� ������� ������ ���� /usr/local/bin/backup.sh
	
	LOG="/var/log/borg/backup.log"
	BACKUP_USER="u602"
	REPOSITORY_DIR="/Repository"
	SERVER="192.168.11.102"
	BACKUP_COMMAND="borg@${SERVER}:${REPOSITORY_DIR}"
	export PATH=${PATH}:/usr/local/bin
	exec > >(tee -i ${LOG})
	exec 2>&1
	echo "###### Backup started: $(date) ######"
	borg create -v --stats ${BACKUP_COMMAND}::'{now:%Y-%m-%d_%H:%M}'  /etc
	borg prune -v --dry-run --keep-daily=7 $BACKUP_COMMAND 

�������� ������� cron �� ������� ��� � 10 �����
	
	crontab -l
	*/10 * * * * /usr/local/bin/backup.sh

� ���������� �� ������� ������� ���� ��������
 
	/usr/local/bin/borg list borg@192.168.11.102:/Repository
	2019-10-10_09:10                     Thu, 2019-10-10 09:10:03 [77383160e57a23119582c7a91d0e27bd3981ee622ec7e7460e8f79684e854666]
	2019-10-10_09:20                     Thu, 2019-10-10 09:20:02 [0b3ce38edfd22ac8d3a098c1b58fc922bdf428328be945d2800441ee42ef397c]
	2019-10-10_09:30                     Thu, 2019-10-10 09:30:02 [3085f27e45906dc3a7f908368a1ff385f46f84d3b06830e12bdb292493482ab9]
	2019-10-10_09:40                     Thu, 2019-10-10 09:40:02 [38d1dc93f8a59042fcd09de7768f80e535a4f4734f4b321219900a272491faff]
	2019-10-10_09:50                     Thu, 2019-10-10 09:50:02 [11cf71a338d0b45a5412ab47b46fcf6e605a7be3633df73e7fa0dcef41d98763]
	2019-10-10_10:00                     Thu, 2019-10-10 10:00:02 [52e5b7fd2dd3ab38d11f5d8b69a6323ba8e011df4c29d4fda3cac424c8c58fbd]
	2019-10-10_10:10                     Thu, 2019-10-10 10:10:03 [204c8977a4cc8bab0818db99ad453e7c4ad09cf3382ccbe204608a1f36fff2d6]


