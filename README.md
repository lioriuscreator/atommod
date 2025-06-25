// Atommod – by lioriuscreator

elements.atombomb = {
    color: ["#777777", "#999999", "#555555"],
    behavior: behaviors.SOLID,
    category: "weapons",
    state: "solid",
    density: 9000,
    excludeRandom: true,

    reactions: {
        "fire": {
            elem1: null,
            func: function(pixel) {
                if (!pixel.fired) {
                    pixel.fired = true;
                    explodeNuke(pixel);
                }
            }
        },
        "plasma": {
            elem1: null,
            func: function(pixel) {
                if (!pixel.fired) {
                    pixel.fired = true;
                    explodeNuke(pixel);
                }
            }
        }
    }
};

function explodeNuke(pixel) {
    const radius = 55;
    const blast = 300;
    const heat = 20000;
    const x = pixel.x;
    const y = pixel.y;

    // große Explosion
    explodeAt(x, y, blast);

    // Hitze und Effekte im Umkreis
    for (let dx = -radius; dx <= radius; dx++) {
        for (let dy = -radius; dy <= radius; dy++) {
            let dist = Math.hypot(dx, dy);
            if (dist <= radius) {
                let nx = x + dx;
                let ny = y + dy;
                if (!isEmpty(nx, ny)) {
                    if (Math.random() < 0.4) {
                        changePixel(nx, ny, "fire");
                    } else if (Math.random() < 0.2) {
                        changePixel(nx, ny, "plasma");
                    }
                    let p = currentPixels[nx]?.[ny];
                    if (p) {
                        p.temp += heat;
                    }
                }
            }
        }
    }

    // Pilzwolke (optional)
    for (let i = 0; i < 30; i++) {
        createPixel("smoke", x + Math.floor(Math.random() * 10 - 5), y - i);
    }

    // Bombe verschwindet
    deletePixel(x, y);
}
