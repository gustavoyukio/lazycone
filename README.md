lazycone
========

Unix code for fast cloning git hub projects

lazyclone(){
    # $1 = www_dir
    # $2 = github user
    # $3 = project name
    # $4 = framework
    # Start $1 = www_dir
	mkdir -p $1/$3
	cd $1/$3

	# Layouts
	mkdir layouts
	mkdir layouts/images
	mkdir layouts/psd

	# Development
	mkdir projeto
	mkdir init
	mkdir db

	# provas 
	mkdir proof
	
	# Estou na pasta www_dir/$3
	git clone https://github.com/$1/$2 projeto
	
	# Checando os frameworks
	if [ "$4" == "cake" ]
	then
		cd projeto
		mkdir -p app/tmp
		mkdir -p app/tmp/log
		mkdir -p app/tmp/logs
		mkdir -p app/tmp/cache
		mkdir -p app/tmp/cache/persistent
		mkdir -p app/tmp/cache/models
		mkdir -p app/tmp/cache/views
		mkdir -p app/tmp/cache/css
		mkdir -p app/tmp/cache/js
		mkdir -p app/tmp/cache/controllers
		
		sudo chmod -R 777 app/tmp
	elif [ "$4" == "zend" ]
	then
		#criar pastas
		mkdir application/models/proxies
	fi

	# precisamos criar o arquivo de vhost
	echo "Primiero criar um arquivo com o nome do site em sites-avaiable"
    
    echo "Entramos no sites available"
    cd /etc/apache2/sites-available
	sudo echo "
		<VirtualHost *:80>
			DocumentRoot /home/gustavo/Desktop/www/$3
			ServerName $3.local
			ServerAlias $3.local
	
			<Directory /home/gustavo/Desktop/www/$3>
				Options Includes FollowSymLinks
				AllowOverride All
				Order allow,deny
				Allow from all
			</Directory>
		</VirtualHost>
		" >> vhost
    echo "Arquivo Criado"
    #adicionamos no hosts
    echo "127.0.0.1 $3.local" | sudo tee -a /etc/hosts
    
    # Restart the Server
	echo "depois adicionar o vm nele, entar no sites enabled e usar o comando a2ensite arquivo, e restart o apache"
    sudo /etc/init.d/apache2 restart
    
    #retornando para a pasta
    cd ~/Desktop/www/$3
}
