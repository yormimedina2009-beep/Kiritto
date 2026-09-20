-- ============================================
-- SERVER HOP con filtro de jugadores
-- Interfaz arrastrable
-- ============================================

repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local StarterGui = game:GetService("StarterGui")
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- Evitar duplicados
if CoreGui:FindFirstChild("ServerHopGui") then
    CoreGui.ServerHopGui:Destroy()
end

-- ============================================
-- GUI PRINCIPAL
-- ============================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ServerHopGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = CoreGui

-- Ventana
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.new(0, 300, 0, 290)
Main.Position = UDim2.new(0, 30, 0.5, -145)
Main.BackgroundColor3 = Color3.fromRGB(15, 17, 23)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Main.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(0, 212, 255)
MainStroke.Thickness = 1
MainStroke.Transparency = 0.5
MainStroke.Parent = Main

-- ============================================
-- BARRA SUPERIOR (arrastrable)
-- ============================================
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 38)
TopBar.BackgroundColor3 = Color3.fromRGB(20, 23, 30)
TopBar.BorderSizePixel = 0
TopBar.Active = true
TopBar.Parent = Main

local TopCorner = Instance.new("UICorner")
TopCorner.CornerRadius = UDim.new(0, 12)
TopCorner.Parent = TopBar

local TopFix = Instance.new("Frame")
TopFix.Size = UDim2.new(1, 0, 0, 15)
TopFix.Position = UDim2.new(0, 0, 1, -15)
TopFix.BackgroundColor3 = Color3.fromRGB(20, 23, 30)
TopFix.BorderSizePixel = 0
TopFix.Parent = TopBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -50, 1, 0)
Title.Position = UDim2.new(0, 12, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "🔁 SERVER HOP"
Title.TextColor3 = Color3.fromRGB(0, 212, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 26, 0, 26)
CloseBtn.Position = UDim2.new(1, -32, 0, 6)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 12
CloseBtn.Parent = TopBar

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseBtn

-- ============================================
-- CONTENIDO
-- ============================================
local InfoLabel = Instance.new("TextLabel")
InfoLabel.Size = UDim2.new(1, -20, 0, 20)
InfoLabel.Position = UDim2.new(0, 10, 0, 48)
InfoLabel.BackgroundTransparency = 1
InfoLabel.Text = "Jugadores máx. en el servidor:"
InfoLabel.TextColor3 = Color3.fromRGB(200, 205, 215)
InfoLabel.Font = Enum.Font.Gotham
InfoLabel.TextSize = 12
InfoLabel.TextXAlignment = Enum.TextXAlignment.Left
InfoLabel.Parent = Main

local TextBox = Instance.new("TextBox")
TextBox.Size = UDim2.new(1, -20, 0, 38)
TextBox.Position = UDim2.new(0, 10, 0, 72)
TextBox.BackgroundColor3 = Color3.fromRGB(26, 29, 41)
TextBox.BorderSizePixel = 0
TextBox.Text = "0"
TextBox.PlaceholderText = "Ej: 0 para vacío, 5 para 5 jugadores"
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.PlaceholderColor3 = Color3.fromRGB(120, 125, 140)
TextBox.Font = Enum.Font.Gotham
TextBox.TextSize = 14
TextBox.ClearTextOnFocus = false
TextBox.Parent = Main

local BoxCorner = Instance.new("UICorner")
BoxCorner.CornerRadius = UDim.new(0, 8)
BoxCorner.Parent = TextBox

local BoxStroke = Instance.new("UIStroke")
BoxStroke.Color = Color3.fromRGB(0, 212, 255)
BoxStroke.Thickness = 1
BoxStroke.Transparency = 0.5
BoxStroke.Parent = TextBox

-- Botones rápidos
local QuickFrame = Instance.new("Frame")
QuickFrame.Size = UDim2.new(1, -20, 0, 30)
QuickFrame.Position = UDim2.new(0, 10, 0, 118)
QuickFrame.BackgroundTransparency = 1
QuickFrame.Parent = Main

local QuickLayout = Instance.new("UIListLayout")
QuickLayout.FillDirection = Enum.FillDirection.Horizontal
QuickLayout.Padding = UDim.new(0, 6)
QuickLayout.Parent = QuickFrame

local function crearQuickBtn(texto, valor)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0, 62, 0, 28)
    b.BackgroundColor3 = Color3.fromRGB(30, 35, 48)
    b.Text = texto
    b.TextColor3 = Color3.fromRGB(200, 205, 215)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 12
    b.Parent = QuickFrame
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
    b.MouseButton1Click:Connect(function()
        TextBox.Text = tostring(valor)
    end)
end

crearQuickBtn("Vacío", 0)
crearQuickBtn("1", 1)
crearQuickBtn("5", 5)
crearQuickBtn("10", 10)

-- Botón principal
local HopButton = Instance.new("TextButton")
HopButton.Size = UDim2.new(1, -20, 0, 42)
HopButton.Position = UDim2.new(0, 10, 0, 158)
HopButton.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
HopButton.Text = "🔍 BUSCAR Y UNIRSE"
HopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
HopButton.Font = Enum.Font.GothamBold
HopButton.TextSize = 13
HopButton.Parent = Main

local HopCorner = Instance.new("UICorner")
HopCorner.CornerRadius = UDim.new(0, 8)
HopCorner.Parent = HopButton

-- Estado
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, -20, 0, 40)
StatusLabel.Position = UDim2.new(0, 10, 0, 210)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "Listo para buscar."
StatusLabel.TextColor3 = Color3.fromRGB(160, 165, 180)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 11
StatusLabel.TextWrapped = true
StatusLabel.TextXAlignment = Enum.TextXAlignment.Left
StatusLabel.TextYAlignment = Enum.TextYAlignment.Top
StatusLabel.Parent = Main

local Credit = Instance.new("TextLabel")
Credit.Size = UDim2.new(1, -20, 0, 18)
Credit.Position = UDim2.new(0, 10, 1, -22)
Credit.BackgroundTransparency = 1
Credit.Text = "Kiritto Hub"
Credit.TextColor3 = Color3.fromRGB(0, 212, 255)
Credit.Font = Enum.Font.Gotham
Credit.TextSize = 10
Credit.TextXAlignment = Enum.TextXAlignment.Right
Credit.Parent = Main

-- ============================================
-- FUNCIONES
-- ============================================
local function notificar(titulo, texto)
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = titulo,
            Text = texto,
            Duration = 4
        })
    end)
end

local function setStatus(txt, color)
    StatusLabel.Text = txt
    StatusLabel.TextColor3 = color or Color3.fromRGB(160, 165, 180)
end

-- Obtiene servidores en varias páginas para tener más opciones
local function obtenerServidores(placeId)
    local todos = {}
    local cursor = ""
    
    for i = 1, 3 do
        local url = "https://games.roblox.com/v1/games/" .. placeId ..
                    "/servers/Public?sortOrder=Asc&limit=100"
        if cursor ~= "" then
            url = url .. "&cursor=" .. cursor
        end
        
        local ok, respuesta = pcall(function()
            return game:HttpGet(url)
        end)
        if not ok then break end
        
        local ok2, datos = pcall(function()
            return HttpService:JSONDecode(respuesta)
        end)
        if not ok2 or not datos or not datos.data then break end
        
        for _, s in ipairs(datos.data) do
            if s.id ~= game.JobId and s.playing < s.maxPlayers then
                table.insert(todos, s)
            end
        end
        
        cursor = datos.nextPageCursor or ""
        if cursor == "" then break end
        
        task.wait(0.25)
    end
    
    return todos
end

-- ============================================
-- LÓGICA PRINCIPAL
-- ============================================
local function buscarYUnirse()
    local numeroDeseado = tonumber(TextBox.Text)
    
    if not numeroDeseado or numeroDeseado < 0 then
        setStatus("❌ Ingresa un número válido (0 o mayor).", Color3.fromRGB(255, 100, 100))
        notificar("Error", "Número inválido")
        return
    end
    
    HopButton.Text = "⏳ BUSCANDO..."
    HopButton.BackgroundColor3 = Color3.fromRGB(120, 120, 0)
    setStatus("Obteniendo lista de servidores...", Color3.fromRGB(255, 200, 0))
    
    local placeId = game.PlaceId
    local servidores = obtenerServidores(placeId)
    
    if #servidores == 0 then
        setStatus("❌ No se encontraron servidores disponibles.", Color3.fromRGB(255, 100, 100))
        HopButton.Text = "🔍 BUSCAR Y UNIRSE"
        HopButton.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
        return
    end
    
    -- Ordenar de menor a mayor
    table.sort(servidores, function(a, b)
        return a.playing < b.playing
    end)
    
    -- 1) Buscar coincidencia EXACTA
    local elegido = nil
    for _, s in ipairs(servidores) do
        if s.playing == numeroDeseado then
            elegido = s
            break
        end
    end
    
    -- 2) Si no hay exacto, el más cercano SIN SUPERARLO
    if not elegido then
        for _, s in ipairs(servidores) do
            if s.playing <= numeroDeseado then
                elegido = s
            end
        end
    end
    
    -- 3) Si todos tienen más, avisar en vez de meter a uno lleno
    if not elegido then
        setStatus("⚠️ No hay servidores con ≤ " .. numeroDeseado .. " jugadores.\nEl más vacío tiene " .. servidores[1].playing .. ".", Color3.fromRGB(255, 200, 0))
        notificar("Sin coincidencia", "Todos tienen más de " .. numeroDeseado .. " jugadores.")
        HopButton.Text = "🔍 BUSCAR Y UNIRSE"
        HopButton.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
        return
    end
    
    setStatus("✅ Uniendo a servidor con " .. elegido.playing .. "/" .. elegido.maxPlayers .. " jugadores...", Color3.fromRGB(0, 220, 100))
    notificar("Servidor encontrado", "Uniendo a uno con " .. elegido.playing .. " jugadores...")
    
    task.wait(0.6)
    
    -- Teleport (con reintento)
    local ok, err = pcall(function()
        TeleportService:TeleportToPlaceInstance(placeId, elegido.id, LocalPlayer)
    end)
    
    if not ok then
        pcall(function()
            TeleportService:Teleport(placeId, LocalPlayer, elegido.id)
        end)
    end
    
    HopButton.Text = "🔍 BUSCAR Y UNIRSE"
    HopButton.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
end

HopButton.MouseButton1Click:Connect(buscarYUnirse)

-- ============================================
-- CERRAR
-- ============================================
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- ============================================
-- NOTIFICACIÓN INICIAL
-- ============================================
task.wait(0.5)
notificar("🔁 Server Hop", "GUI lista. Arrástrala donde quieras.")
print("✅ Server Hop cargado")
