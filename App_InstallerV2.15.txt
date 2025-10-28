# Define an array of package IDs.
$packages = @(
    "Notepad++.Notepad++",
    "Microsoft.VisualStudioCode",
    "Google.Chrome",
    "7zip.7zip",
    "Mozilla.Firefox",
    "Winamp.Winamp",
    "Corel.WinZip",
    "Kingsoft.WPSOffice",
    "VideoLAN.VLC",
    "Microsoft.VisualStudio.2022.Community.Preview",
    "SublimeHQ.SublimeText.3",
    "Meltytech.Shotcut",
    "JetBrains.PyCharm.Community",
    "Prusa3D.PrusaSlicer",
    "Daum.PotPlayer",
    "Wondershare.Filmora",
    "OpenShot.OpenShot",
    "KDE.Kdenlive",
    "Inkscape.Inkscape",
    "BlenderFoundation.Blender",
    "dotPDN.PaintDotNet",
    "GIMP.GIMP.2",
    "Git.Git",
    "RARLab.WinRAR",
    "XBMCFoundation.Kodi",
    "LibreCAD.LibreCAD",
    "TheDocumentFoundation.LibreOffice",
    "Adobe.Acrobat.Reader.64-bit",
    "JGraph.Draw",
    "gnome.Dia",
    "Microsoft.VisioViewer",
    "JetBrains.IntelliJIDEA.Community",
    "GodotEngine.GodotEngine",
    "Microsoft.PowerToys",
    "Celestia.Celestia",
    "TheQucsTeam.Qucs-S",
    "RubyInstallerTeam.Ruby.3.1",
    "GnuCash.GnuCash",
    "KINMEAG.KNIMEAnalyticsPlatform",
    "Evolus.Pencil"
)

foreach ($package in $packages) {
    # Escape the package name to handle any regex special characters
    $escapedPackage = [Regex]::Escape($package)
    
    # Capture the output of 'winget list' for the specific package.
    $installedApp = winget list --id $package | Out-String

    if ($installedApp -match $escapedPackage) {
        Write-Host "$package is already installed. Skipping."
    } else {
        Write-Host "Installing $package"
        if ($package -eq "GodotEngine.GodotEngine") {
            # For Godot we use additional parameters (for ARM64, etc.)
            winget install --id $package --source winget --architecture arm64 --accept-source-agreements --accept-package-agreements
        } else {
            winget install --id=$package -e --accept-source-agreements --accept-package-agreements
        }
    }
}

# Optionally, upgrade any outdated apps.
winget upgrade --all

