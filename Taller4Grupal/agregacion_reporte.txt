const DiasTardadoPorInspeccionDeReporte = [
  {
    $lookup: {
      from: "inspecciones",
      localField: "reporte_id",
      foreignField: "reporte_id",
      as: "inspecciones"
    }
  },
  {
    $unwind: "$inspecciones"
  },
  {
    $project: {
      _id: 0,
      reporte_id: 1,
      titulo: 1,
      fecha_reporte: 1,
      inspector: "$inspecciones.inspector",
      fecha_visita: "$inspecciones.fecha_visita",
      resultado: "$inspecciones.resultado",
      dias_transcurridos: {
        $dateDiff: {
          startDate: "$fecha_reporte",
          endDate: "$inspecciones.fecha_visita",
          unit: "day"
        }
      }
    }
  },
  {
    $sort: {
      reporte_id: 1,
      fecha_visita: 1
    }
  }
]
