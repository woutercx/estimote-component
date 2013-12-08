require "rake/clean"

CLEAN.include "*.xam"
CLEAN.include "xpkg"
CLEAN.include "source/binding/*.zip"
CLEAN.include "source/binding/*.dll"
CLEAN.include "source/binding/bin"
CLEAN.include "source/binding/obj"
CLEAN.include "source/samples/EstimoteSample.iOS/bin"
CLEAN.include "source/samples/EstimoteSample.iOS/obj"

COMPONENT = "estimote-component-1.0.0.xam"
MONOXBUILD = "/Library/Frameworks/Mono.framework/Commands/xbuild"

file "xpkg/xamarin-component.exe" do
	puts "* Downloading xamarin-component..."
	mkdir "xpkg"
	sh "curl -L https://components.xamarin.com/submit/xpkg > xpkg.zip"
	sh "unzip -o -q xpkg.zip -d xpkg"
	sh "rm xpkg.zip"
end

file "source/binding/Estimote.iOS.dll" do
  sh "#{MONOXBUILD} /p:Configuration=Release source/binding/Estimote.iOS.csproj"
  sh "cp source/binding/bin/Release/Estimote.iOS.dll source/binding/Estimote.iOS.dll"
end

task :default => ["xpkg/xamarin-component.exe", "source/binding/Estimote.iOS.dll"] do
	line = <<-END
	mono xpkg/xamarin-component.exe package
	END
	puts "* Creating #{COMPONENT}..."
	puts line.strip.gsub "\t\t", "\\\n    "
	sh line, :verbose => false
	puts "* Created #{COMPONENT}"
end

task :prepare => "source/binding/Estimote.iOS.dll" do
  puts "\n\n"
  puts "Binding project prepared, now you can open Estimote.iOS.csproj and samples in Xamarin Studio"
end
