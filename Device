-- ==========================================
-- GHOST_MODZ + TV DUCRA
-- ==========================================

function mostrarCreditos()
    gg.toast("👑 CRIADORES: GHOST_MODZ + TV DUCRA MODZ")
end

mostrarCreditos()

-- ==========================================
-- VARIÁVEIS GLOBAIS E CONFIGURAÇÕES
-- ==========================================
local BASE = -4411604134709397577
local OX, OY, OZ = 0x4, 0x8, 0x0
local addrX, addrY, addrZ = nil, nil, nil
local ENDERECO_BASE = nil

local TEMPO_ESPERA_PADRAO = 6  -- 6 segundos para CPs 1 a 6
local TEMPO_ESPERA_FINAL = 1   -- 1 segundo para o CP 7
local SPEED_FAZENDA = 0.5      -- Velocidade para deslizar devagar
local SPEED_MINA = 6.0   
local DIST_MIN = 1.5           -- Precisão para detectar a chegada no ponto

-- ==========================================
-- FUNÇÕES DE BUSCA E BASE
-- ==========================================
function encontrarBase()
    gg.clearResults()
    gg.setRanges (gg.REGION_OTHER)
    gg.toast("🔍 Buscando base do jogador...")
    gg.searchNumber(tostring(BASE), gg.TYPE_QWORD)
    local r = gg.getResults(3)
    if #r == 0 then 
        gg.toast("❌ Base não encontrada") 
        return false 
    end

    local c1, c2 = {}, {}
    for i, v in ipairs(r) do
        c1[i] = {address = v.address - 208, flags = gg.TYPE_QWORD}
        c2[i] = {address = v.address - 212, flags = gg.TYPE_QWORD}
    end
    c1 = gg.getValues(c1)
    c2 = gg.getValues(c2)

    for i = 1, #c1 do
        if c1[i].value == -4411463732228264604 and c2[i].value == -4250292608395772511 then
            ENDERECO_BASE = c1[i].address - 108
            addrX = ENDERECO_BASE + OX
            addrY = ENDERECO_BASE + OY
            addrZ = ENDERECO_BASE + OZ
            gg.toast("✅ Player conectado com sucesso!")
            return true
        end
    end
    gg.toast("❌ Falha ao calibrar base")
    return false
end

function TP(x, y, z)
    if not addrX or not addrY or not addrZ then
        if not encontrarBase() then return end
    end
    gg.setValues({
        {address = addrX, value = x, flags = gg.TYPE_FLOAT},
        {address = addrY, value = y, flags = gg.TYPE_FLOAT},
        {address = addrZ, value = z, flags = gg.TYPE_FLOAT}
    })
    gg.toast("✅ Teleportado!")
end

-- ==========================================
-- FUNÇÕES DO MENU PLAYER
-- ==========================================
function setHealth(value)
    gg.clearResults()
    gg.searchNumber("99999.9921875", gg.TYPE_FLOAT)
    local res = gg.getResults(50)
    if #res == 0 then return gg.toast("❌ Endereço de vida não localizado") end
    local addr = res[1].address - 0x54
    gg.setValues({{address = addr, flags = gg.TYPE_FLOAT, value = value}})
    gg.toast("❤️ Vida alterada para: " .. value)
end

function setArmor(value)
    gg.clearResults()
    gg.searchNumber("99999.9921875", gg.TYPE_FLOAT)
    local res = gg.getResults(30)
    if #res == 0 then return gg.toast("❌ Endereço de colete não localizado") end
    local addr = res[1].address - 0x4C
    gg.setValues({{address = addr, flags = gg.TYPE_FLOAT, value = value}})
    gg.toast("🛡️ Colete alterado para: " .. value)
end

function SpeedHack(multi)
    gg.clearResults()
    gg.searchNumber("3.14159274101", gg.TYPE_FLOAT)
    local res = gg.getResults(25)
    if #res == 0 then return gg.toast("❌ Speed não encontrado") end
    for i, v in ipairs(res) do
        gg.setValues({{address = v.address - 0x24, flags = gg.TYPE_FLOAT, value = multi}})
    end
    gg.toast("⚡ Speed Modificado para: " .. multi .. "x")
end

function ativarSuperPulo(multi)
    gg.clearResults()
    gg.searchNumber("4.15768349e21", gg.TYPE_FLOAT)
    local res = gg.getResults(3000)
    for i, v in ipairs(res) do
        gg.setValues({{address = v.address - 36, flags = gg.TYPE_FLOAT, value = multi}})
    end
    gg.toast("🦘 Pulo Modificado para: " .. multi .. "x")
end

-- ==========================================
-- NOVO SISTEMA DE FARM FAZENDA GRAVÁVEL (7 CPs)
-- ==========================================
function farmFazendaBot()
    if not addrZ and not encontrarBase() then return end
    
    local checkpointsSalvos = {}
    local maxCheckpoints = 7
    
    -- FASE 1: Gravação das Coordenadas
    gg.alert("🎯 MODO GRAVAÇÃO\n\nAnde até cada ponto e clique no ícone do GG para salvar as posições de 1 a 7.\nNa 8ª vez, o bot iniciará.", "ENTENDIDO")
    
    while #checkpointsSalvos < maxCheckpoints do
        if gg.isVisible() then
            gg.setVisible(false)
            
            local pos = gg.getValues({
                {address = addrX, flags = gg.TYPE_FLOAT}, 
                {address = addrY, flags = gg.TYPE_FLOAT}, 
                {address = addrZ, flags = gg.TYPE_FLOAT}
            })
            
            local numCP = #checkpointsSalvos + 1
            table.insert(checkpointsSalvos, {x = pos[1].value, y = pos[2].value, z = pos[3].value})
            
            gg.toast("📍 Posição " .. numCP .. " salva com sucesso!")
        end
        gg.sleep(100)
    end
    
    -- Aguarda o 8º clique para iniciar
    gg.toast("✅ 7 pontos salvos! Clique mais uma vez no GG para INICIAR o Farm.")
    while true do
        if gg.isVisible() then
            gg.setVisible(false)
            break
        end
        gg.sleep(100)
    end

    -- FASE 2: Execução do Farm
    local farmFazendaAtivo = true
    local cp = 1
    gg.toast("🌾 Farm Fazenda Iniciado com sua rota!")

    while farmFazendaAtivo do
        if gg.isVisible() then
            gg.setVisible(false)
            local escolha = gg.alert("🌾 FAZENDA AUTOMÁTICA", "🛑 PARAR FARM", "⏭ PULAR PONTO")
            if escolha == 1 then 
                farmFazendaAtivo = false
                break 
            elseif escolha == 2 then
                cp = cp + 1
                if cp > maxCheckpoints then cp = 1 end
            end
        end

        local alvo = checkpointsSalvos[cp]
        local pos = gg.getValues({
            {address = addrX, flags = gg.TYPE_FLOAT}, 
            {address = addrY, flags = gg.TYPE_FLOAT}, 
            {address = addrZ, flags = gg.TYPE_FLOAT}
        })
        
        local dx, dy, dz = alvo.x - pos[1].value, alvo.y - pos[2].value, alvo.z - pos[3].value
        local dist = math.sqrt(dx*dx + dy*dy + dz*dz)

        if dist < DIST_MIN then
            local tempoEspera = (cp == maxCheckpoints) and TEMPO_ESPERA_FINAL or TEMPO_ESPERA_PADRAO
            gg.toast("🌱 Chegou na Posição " .. cp .. "! Esperando " .. tempoEspera .. "s...")
            
            gg.sleep(tempoEspera * 1000)
            
            cp = cp + 1
            if cp > maxCheckpoints then cp = 1 end
        else
            local nx = pos[1].value + (dx / dist) * SPEED_FAZENDA
            local ny = pos[2].value + (dy / dist) * SPEED_FAZENDA
            local nz = pos[3].value + (dz / dist) * SPEED_FAZENDA
            
            gg.setValues({
                {address = addrX, value = nx, flags = gg.TYPE_FLOAT}, 
                {address = addrY, value = ny, flags = gg.TYPE_FLOAT}, 
                {address = addrZ, value = nz, flags = gg.TYPE_FLOAT}
            })
        end
        gg.sleep(50)
    end
end

-- ==========================================
-- SISTEMA DE MINA ANTERIOR
-- ==========================================
local checkpointsMina = {
    {nome = "ENTRADA",   x = 19.4, y = 1012.2, z = 859.3},
    {nome = "CORREDOR",  x = 21.1, y = 1012.1, z = 833.7},
    {nome = "FUNDO",     x = 23.2, y = 1009.6, z = 807.9},
    {nome = "VOLTA",     x = 21.0, y = 1012.0, z = 833.5},
    {nome = "REINICIO",  x = 19.2, y = 1012.3, z = 859.5}
}

function farmMinaBot()
    if not addrZ and not encontrarBase() then return end
    local farmMinaAtivo = true
    local cp = 1
    gg.toast("⛏️ Farm Mina Iniciado!")

    while farmMinaAtivo do
        if gg.isVisible() then
            gg.setVisible(false)
            local escolha = gg.alert("⛏️ MINA ATIVA", "🛑 PARAR FARM", "⏭ PULAR CP")
            if escolha == 1 then 
                farmMinaAtivo = false
                break 
            elseif escolha == 2 then
                cp = cp + 1
                if cp > #checkpointsMina then cp = 1 end
            end
        end

        local posY = gg.getValues({{address = addrY, flags = gg.TYPE_FLOAT}})[1].value
        if posY < 900 then
            TP(checkpointsMina[1].x, checkpointsMina[1].y, checkpointsMina[1].z)
            cp = 1
        end

        local alvo = checkpointsMina[cp]
        local pos = gg.getValues({{address = addrX, flags = gg.TYPE_FLOAT}, {address = addrY, flags = gg.TYPE_FLOAT}, {address = addrZ, flags = gg.TYPE_FLOAT}})
        local dx, dy, dz = alvo.x - pos[1].value, alvo.y - pos[2].value, alvo.z - pos[3].value
        local dist = math.sqrt(dx*dx + dy*dy + dz*dz)

        if dist < DIST_MIN then
            gg.toast("⛏️ Coletando em: " .. alvo.nome)
            gg.sleep(6000)
            cp = cp + 1
            if cp > #checkpointsMina then cp = 1 end
        else
            local nx = pos[1].value + (dx / dist) * SPEED_MINA
            local ny = pos[2].value + (dy / dist) * SPEED_MINA
            local nz = pos[3].value + (dz / dist) * SPEED_MINA
            gg.setValues({{address = addrX, value = nx, flags = gg.TYPE_FLOAT}, {address = addrY, value = ny, flags = gg.TYPE_FLOAT}, {address = addrZ, value = nz, flags = gg.TYPE_FLOAT}})
        end
        gg.sleep(70)
    end
end

-- ==========================================
-- SISTEMA: SAIR DA PRISÃO (COM COORDENADAS COLETADAS)
-- ==========================================
function sairDaPrisaoBot()
    if not addrZ and not encontrarBase() then return end

    -- Coordenadas reais injetadas com sucesso!
    local x1, y1, z1 = 1140.140, 2159.920, 1752.780   -- Ponto 1
    local x2, y2, z2 = -2797.580, 32.894, -16.028     -- Ponto 2
    local x3, y3, z3 = -2117.915, 9.087, 355.186      -- Ponto 3

    local RAIO_DETECCAO = 2.0 -- Raio de proximidade em metros para o Ponto 2

    gg.toast("🏃 Iniciando Fuga: Indo para o Ponto 1...")
    TP(x1, y1, z1)
    gg.sleep(500)

    gg.toast("🔍 Monitorando aproximação do Ponto 2 (Sistema de Raio)...")
    local monitorando = true

    while monitorando do
        if gg.isVisible() then
            gg.setVisible(false)
            if gg.alert("🏃 FUGA ATIVA", "🛑 PARAR FUGA") == 1 then
                monitorando = false
                break
            end
        end

        local pos = gg.getValues({
            {address = addrX, flags = gg.TYPE_FLOAT}, 
            {address = addrY, flags = gg.TYPE_FLOAT}, 
            {address = addrZ, flags = gg.TYPE_FLOAT}
        })

        local dx = x2 - pos[1].value
        local dy = y2 - pos[2].value
        local dz = z2 - pos[3].value
        local dist = math.sqrt(dx*dx + dy*dy + dz*dz)

        -- Se entrar no raio do Ponto 2, vai instantaneamente para o Ponto 3
        if dist <= RAIO_DETECCAO then
            gg.toast("⚡ Raio atingido! Teleportando para o Ponto 3 (Liberdade)!")
            TP(x3, y3, z3)
            monitorando = false
            break
        end

        gg.sleep(100)
    end
end

-- ==========================================
-- FUNÇÃO TELEPORTE GPS (CORRIGIDA)
-- ==========================================
function TP_GPS()
    if not addrX and not encontrarBase() then return end
    
    gg.clearResults()
    gg.setRanges(gg.REGION_C_BSS)
    gg.searchNumber("7233187898168705024", gg.TYPE_QWORD)
    local r = gg.getResults(100)

    if #r == 0 then
        gg.toast("❌ Marque um destino no mapa primeiro!")
        return
    end

    local gps_address = nil
    for i, v in ipairs(r) do
        local test = gg.getValues({{address = v.address - 0x14, flags = gg.TYPE_FLOAT}})[1].value
        if test ~= 0 then 
            gps_address = v.address
            break 
        end
    end

    if not gps_address then 
        return gg.toast("❌ GPS não encontrado ou inválido") 
    end

    local gpsAtivo = true
    gg.toast("📍 Teleporte GPS Ligado")

    while gpsAtivo do
        if gg.isVisible() then
            gg.setVisible(false)
            if gg.alert("📍 TP GPS ATIVO", "🛑 PARAR") == 1 then 
                gpsAtivo = false
                break 
            end
        end

        local atual = gg.getValues({
            {address = gps_address - 0x14, flags = gg.TYPE_FLOAT}, -- Z
            {address = gps_address - 0x10, flags = gg.TYPE_FLOAT}, -- X
            {address = gps_address - 0x0C, flags = gg.TYPE_FLOAT}  -- Y
        })

        if atual[1].value ~= 0 and atual[2].value ~= 0 then
            gg.setValues({
                {address = addrX, value = atual[2].value, flags = gg.TYPE_FLOAT},
                {address = addrY, value = atual[3].value, flags = gg.TYPE_FLOAT},
                {address = addrZ, value = atual[1].value, flags = gg.TYPE_FLOAT}
            })
        end
        gg.sleep(300)
    end
end

-- ==========================================
-- ESTRUTURA DOS MENUS EM ABAS
-- ==========================================

function menuPlayer()
    local escolha = gg.choice({
        "⌨️ Setar vida",
        "❤️ Vida: 200",
        "❤️ Vida: 100",
        "⌨️ Setar colete",
        "🛡️ Colete: 200",
        "🛡️ Colete: 100",
        "⚡ Speed 8x",
        "⚡ Speed 5x",
        "⚡ Speed 1x",
        "🦘 Pulo 8x",
        "🦘 Pulo 5x",
        "🦘 Pulo 1x",
        "↩️ retorna"
    }, nil, "👤 OPÇÕES DO PLAYER")

    if escolha == 1 then
        local input = gg.prompt({"Valor da Vida:"}, {"9999"}, {"number"})
        if input then setHealth(tonumber(input[1])) end
    elseif escolha == 2 then setHealth(200)
    elseif escolha == 3 then setHealth(100)
    elseif escolha == 4 then
        local input = gg.prompt({"Valor do Colete:"}, {"9999"}, {"number"})
        if input then setArmor(tonumber(input[1])) end
    elseif escolha == 5 then setArmor(200)
    elseif escolha == 6 then setArmor(100)
    elseif escolha == 7 then SpeedHack(8)
    elseif escolha == 8 then SpeedHack(5)
    elseif escolha == 9 then SpeedHack(1)
    elseif escolha == 10 then ativarSuperPulo(8)
    elseif escolha == 11 then ativarSuperPulo(5)
    elseif escolha == 12 then ativarSuperPulo(1)
    end
end

function menuFarm()
    local escolha = gg.choice({
        "🌾 Farm Fazenda",
        "⛏️ Farm Mina",
        "↩️ Retorna"
    }, nil, "🚜 SISTEMA DE FAZENDAS & MINAS")

    if escolha == 1 then farmFazendaBot()
    elseif escolha == 2 then farmMinaBot()
    end
end

-- ABA GAME (POSICIONADA ABAIXO DA ABA FARM)
function menuGame()
    local escolha = gg.choice({
        "🏃 Sair da Prisão ",
        "↩️ Retorna"
    }, nil, "🎮 MODOS DE JOGO & GAMEPLAY")

    if escolha == 1 then sairDaPrisaoBot() end
end

function menuTeleporte()
    local escolha = gg.choice({
        "📡 Got Gps",
        "🌏 Got locais fixo",
        "↩️ Retorna"
    }, nil, "🗺️ SISTEMA DE TELEPORTE")

    if escolha == 1 then
        TP_GPS()
    elseif escolha == 2 then
        local locais = {
            {"🏝️ ILHA NOVATOS", -1987.606, 8.27, 445.46},
            {"🔫 LOJA ARMA", -971.377, 11.54, -1515.79},
            {"🏢 PREFEITURA", -1858.926, 10.69, 1457.16},
            {"⛽ POSTO", -1374.5211, 20.39, -1001.28},
            {"🍔 LANCHONETE", -1982.1541, 8.20, 499.58},
            {"🚗 ESCOLA CONDUÇÃO", -2002.5773, 8.20, 511.02},
            {"👕 LOJA DE ROUMA", -2018.4404, 8.20, 525.75},
            {"🕶️ MERCADO NEGRO", -2230.20, 40.22, -2511.92},
            {"⛏️ Mina Centro", -1135.10, 43.76, -179.49},
            {"🌾 Fazenda Campo", 190.13, 10.51, -2477.59},
        }
        
        local listaLocais = {}
        for i, loc in ipairs(locais) do table.insert(listaLocais, loc[1]) end
        table.insert(listaLocais, "↩️ Voltar")

        local subEscolha = gg.choice(listaLocais, nil, "🗺️ LOCAIS MAPA")
        if subEscolha and subEscolha ~= #listaLocais then
            local destino = locais[subEscolha]
            TP(destino[2], destino[3], destino[4])
        end
    end
end

function menu_principal()
    local abaPrincipal = gg.choice({
        "•👤 player",
        "•💸 farm",
        "•🎮 game",
        "•🌎 teleport",
        "❌ fechar"
    }, nil, " • CHOST MODZ \n • TV DUCRA ")

    if abaPrincipal == 1 then menuPlayer()
    elseif abaPrincipal == 2 then menuFarm()
    elseif abaPrincipal == 3 then menuGame()
    elseif abaPrincipal == 4 then menuTeleporte()
    elseif abaPrincipal == 5 then os.exit()
    end
end

while true do
    if gg.isVisible(true) then
        gg.setVisible(false)
        menu_principal()
    end
    gg.sleep(100)
end
