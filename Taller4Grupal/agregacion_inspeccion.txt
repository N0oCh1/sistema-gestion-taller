const BuscarPorRegix = [
  {
    $match: {
      observaciones: {
        $regex: "urgente|peligro",
        $options: "i"
      }
    }
  },
  {
    $project: {
      _id: 0,
      reporte_id: 1,
      inspector: 1,
      resultado: 1,
      observaciones: 1
    }
  }
]

const BuscarPorTituloYCorregimiento = [
  {
    $lookup: {
      from: "reportes",
      localField: "reporte_id",
      foreignField: "reporte_id",
      pipeline: [
        {
          $project: {
            _id: 0,
            titulo: 1,
            corregimiento:
              "$ubicacion.corregimiento"
          }
        }
      ],
      as: "reporte"
    }
  },
  {
    $addFields: {
      titulo_reporte: {
        $first: "$reporte.titulo"
      },
      corregimiento: {
        $first: "$reporte.corregimiento"
      }
    }
  },
  { $project: { reporte: 0 } }
]

const ContadorDeInspecciones = [
  {
    $group: {
      _id: "$resultado",
      total: { $sum: 1 }
    }
  },
  { $sort: { total: -1 } }
]