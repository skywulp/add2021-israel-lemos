#!/usr/bin/env ruby

if ARGV.size ==0
  puts "Ejecuta el script con --help"
  exit 1
end


if ARGV[0] =='--help'
  puts "-------------------------------------"
  puts "[INFO] Entrando en la opción de help"
  puts "-------------------------------------"
  puts "Usage:
        systemctml [OPTIONS] [FILENAME]
Options:
        --help, mostrar esta ayuda.
        --version, mostrar información sobre el autor del script
                   y fecha de creacion.
        --status FILENAME, comprueba si puede instalar/desintalar.
        --run FILENAME, instala/desinstala el software indicado.
Description:

        Este script se encarga de instalar/desinstalar
        el software indicado en el fichero FILENAME.
        Ejemplo de FILENAME:
        tree:install
        nmap:install
        atomix:remove"
  exit 1
end

if ARGV[0]=='--version'
  puts "-------------------------------------"
  puts "[INFO] Entrando en la opción de version"
  puts "-------------------------------------"
  puts "autor: Israel Lemos Cruz"
  puts "fecha de creacion: 22/02/2021"
  exit 1
end

if ARGV[0]=='--status'
  puts "-----------------------------------------------------------------"
  puts "[INFO] Entrando en la opción de status con el fichero #{ARGV[1]}"
  puts "-----------------------------------------------------------------"
  a = `cat #{ARGV[1]}`
  b = a.split("\n")
  b.each do |i|
    d=i.split(":")
    c = `zypper info #{d[0]}|grep Instalado|grep Sí |wc -l`.chop
    if c == "1"
      puts "* Estado del paquete #{d[0]} (Instalado)"
    else
      puts "* Estado del paquete #{d[0]} (No instalado)"
    end
  end
end

if ARGV[0]=='--run'
  puts "[INFO] Entrando en la opción de run con el fichero #{ARGV[1]}"

  usuario=`whoami`.chop
  if usuario !="root"
    puts"[ERROR] Tienes que ejecutar la opcion run como root."
    exit 1
  end

  a = `cat #{ARGV[1]}`
  b = a.split("\n")
  b.each do |i|
    d=i.split(":")
    if d[1]=="install"
      puts "----------------------------"
      puts"  Instalar programa #{d[0]}"
      puts "-----------------------------------"
      puts " [INFO] Instalando programa #{d[0]}"
      puts "-----------------------------------"
      system("zypper install -y #{d[0]}") 
      puts "------------------------------------------------"
      puts" [INFO] Programa #{d[0]} instalado correctamente"
      puts "------------------------------------------------"
    else
      d[1]=="remove"
      puts "----------------------------"
      puts"  Borrar programa #{d[0]}"
      puts "--------------------------------"
      puts" [INFO] Borrando programa #{d[0]}"
      puts "--------------------------------"
      system("zypper remove -y  #{d[0]}")
      puts "----------------------------------------------"
      puts" [INFO] Programa #{d[0]} Borrado correctamente"
      puts "----------------------------------------------"
    end
  end
end

